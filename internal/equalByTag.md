# lodash源码分析之equalByTag

本文为读 lodash 源码的第二百一十六篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import eq from '../eq.js'
import equalArrays from './equalArrays.js'
import mapToArray from './mapToArray.js'
import setToArray from './setToArray.js'
```

[《lodash源码分析之eq》](../eq.md)
[《lodash源码分析之equalArrays》](./equalArrays.md)
[《lodash源码分析之mapToArray》](./mapToArray.md)
[《lodash源码分析之setToArray》](./setToArray.md)

## 源码分析

`equalByTag` 会根据 `Object.prototype.toString.call` 得到的结果 `tag` 来判断数据类型，根据不同的数据类型来比较 `object` 和 `other` 是否相等。

`equalByTag` 的入参在[《lodash源码分析之equalArrays》](./equalArrays.md)已经有详细的叙述，这里不再重复。

### 前置知识

在进入源码分析之前，需要温习一下 `switch case` 的一点知识。

如果 `switch` 命中了 `case` 的一个条件，而这个条件又没有写 `break` 或 `return` 语句，则程序会继续执行后面的 `case` 语句，直至遇到第一个 `break` 或者 `return` 语句为止。

如：

```javascript
const type = 1
switch (type) {
  case 1:
    console.log(1)
    break
  case 2:
    console.log(2)
    break
  case 3:
    console.log(3)
    break
}
```

这段代码只会打印出 `1` ，而：

```javascript
const type = 1
switch (type) {
  case 1:
    console.log(1)
  case 2:
    console.log(2)
    break
  case 3:
    console.log(3)
    break
}
```

这段代码会打印出 `1` 和 `2` ，因为 `case 2` 有 `break` 语句，所以执行到这里就终止了。

### 一些常量

```javascript
const COMPARE_PARTIAL_FLAG = 1
const COMPARE_UNORDERED_FLAG = 2

const boolTag = '[object Boolean]'
const dateTag = '[object Date]'
const errorTag = '[object Error]'
const mapTag = '[object Map]'
const numberTag = '[object Number]'
const regexpTag = '[object RegExp]'
const setTag = '[object Set]'
const stringTag = '[object String]'
const symbolTag = '[object Symbol]'

const arrayBufferTag = '[object ArrayBuffer]'
const dataViewTag = '[object DataView]'

const symbolValueOf = Symbol.prototype.valueOf

```

源码开始之前，枚举了将要用到的 `tag` ，保存了 `Symbol.prototype.valueof` 函数。

### 函数主体

```javascript
switch (tag) {
    ...
}
return false
```

可以看到，整个函数就是一个 `switch case` ，如果 `case` 处理完后没有结果，默认返回的是 `false` ，表示不相等。

### 比较 DataView

```javascript
case dataViewTag:
  if ((object.byteLength != other.byteLength) ||
      (object.byteOffset != other.byteOffset)) {
    return false
  }
  object = object.buffer
  other = other.buffer
```

之前的文章有说过，`DataView` 是 `ArrayBuffer` 的视图，因此 `DataView` 相等，首先 `byteLength` 要一致，即所分配的内存长度要一致；`byteOffset` 也要一致，即两个 `DataView` 开始位置的偏移量是否一致。

如果不一致，则返回 `false` 。

比较完这两者，还要比较两者的 `ArrayBuffer` 是否相等。

可以看到，在 `dataViewTag` 这个 `case` 中，只是将两个的 `buffer` 取出，覆盖了原有的变量，但是没有 `break` 语句，这是因为，接下来的 `case` 就是比较 `ArrayBuffer` ，因此将 `ArrayBuffer` 放到下个 `case` 来处理。

### 比较ArrayBuffer

```javascript
case arrayBufferTag:
if ((object.byteLength != other.byteLength) ||
    !equalFunc(new Uint8Array(object), new Uint8Array(other))) {
  return false
}
return true

```

两个 `ArrayBuffer` 的比较，也是先比较内存的大小是否相等，如果不相等，两个 `ArrayBuffer` 肯定不相等了。

如果相等，则使用 `Uint8Array` 视图，传给 `equalFunc` 来比较是否相等。

### 比较Bool、Date及Number 

```javascript
case boolTag:
case dateTag:
case numberTag:
	return eq(+object, +other)
```

`Boolean` 转换成数字时，`true` 会转换成 `1` ，`false` 会转换成 `0` 。

`Date` 转换成数字时，其实相当于 `getTime` ，会转换成对应时间的毫秒数，`Date` 确定，转换成的数字也是确定的。

如果是 `Invalid day` 则会转换成 `NaN` ，`NaN` 在比较时是不相等的。

因此 `Date` 也可以转换成数字来比较。

### 比较Error

```javascript
case errorTag:
	return object.name == other.name && object.message == other.message
```

可以看到，只要 `Error` 对象的 `name` 和 `message` 是相等的，就认为这两个 `Error` 对象相等。

### 比较RegExp及String

```javascript
case regexpTag:
case stringTag:
	return object == `${other}`
```

将正则转换成字符串比较，如果转换后的字符串相等，则认为这两个正则相等。

### 比较Map

```javascript
case mapTag:
	let convert = mapToArray
```

可以看到，这里也没有 `break` ，在这个 `case` 中，也没有进行 `Map` 的比较。

后面会看到，`Map` 和 `Set` 一样，都是将值转换成数组，再比较数组的，因为 `Map` 的比较会在下一个 `case` 中，和 `Set` 的比较使用相同的逻辑。

### 比较Set

```javascript
case setTag:
const isPartial = bitmask & COMPARE_PARTIAL_FLAG
convert || (convert = setToArray)

if (object.size != other.size && !isPartial) {
  return false
}
// Assume cyclic values are equal.
const stacked = stack.get(object)
if (stacked) {
  return stacked == other
}
bitmask |= COMPARE_UNORDERED_FLAG

// Recursively compare objects (susceptible to call stack limits).
stack.set(object, other)
const result = equalArrays(convert(object), convert(other), bitmask, customizer, equalFunc, stack)
stack['delete'](object)
return result
```

可以看到 `Set` 转换成数组的函数使用的是 `setToArray` ，因此会对 `Map` 和 `Set` ，转换函数是不一样的，但最终通过转换函数 `convert` 得到的结果都是数组。

在进行转换和数组比较之前，先比较了两都的长度，如果长度不一致，而又没开启局部比较，则认为两者不相等。

`Map` 和 `Set` 因为值和 `key` 都可能会有复杂的数据结构，因此这里使用 `stack` 来处理循环引用的问题，这个在[《lodash源码分析之equalArrays》](./equalArrays.md)也有了详细的分析，这里的过程是一致的。

因为 `Map` 和 `Set` 通过 `convert` 转换函数后，都是数组，因此，只需要调用 `equalArrays` 比较得到结果即可。

因为 `Map` 和 `Set` 是无序的，因此比较两个数组也要使用无序比较的方式。

### 比较Symbol

```javascript
case symbolTag:
  if (symbolValueOf) {
    return symbolValueOf.call(object) == symbolValueOf.call(other)
  }
```

如果有 `symbolValueOf` 即 `Symbol.prototype.valueOf` ，则使用 `symbolValueOf` 来获取两者的字符串标识，如果两者的标识一致，则认为是相等的。


## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 