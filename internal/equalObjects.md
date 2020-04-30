# lodash源码分析之equalObjects

本文为读 lodash 源码的第二百一十七篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import getAllKeys from './getAllKeys.js'
```

[《lodash源码分析之getAllKeys》](./getAllKeys.md)

## 源码分析

`equalObjects` 用来比较两个对象是否相等。

`equalObjects` 的入参在[《lodash源码分析之equalArrays》](./equalArrays.md)已经有详细的叙述，这里不再重复。

### 一些常量

```javascript
const COMPARE_PARTIAL_FLAG = 1
const hasOwnProperty = Object.prototype.hasOwnProperty
```

`COMPARE_PARTIAL_FLAG` 是用来标记是否局部比较， `hasOwnProperty` 用来保存 `Object.prototype.hasOwnProperty` ，方便后续调用和优化性能。

### 属性长度比较

```javascript
const isPartial = bitmask & COMPARE_PARTIAL_FLAG
const objProps = getAllKeys(object)
const objLength = objProps.length
const othProps = getAllKeys(other)
const othLength = othProps.length

if (objLength != othLength && !isPartial) {
  return false
}
```

首先比较属性的长度，如果长度不一致，又没有开启局部比较，则可以认为这两个对象不相等。

### 属性比较

```javascript
let key
let index = objLength
while (index--) {
  key = objProps[index]
  if (!(isPartial ? key in other : hasOwnProperty.call(other, key))) {
    return false
  }
}
```

上面比较了属性的长度，接下来比较属性，依次取出 `object` 的属性，逐一检查每个属性在 `other` 对象上是否存在。

这里需要注意的是，如果开启局部比较，则会属性在原型链上即可，如果没有开启，则必须是自身的属性。

### 循环依赖比较

```javascript
const stacked = stack.get(object)
if (stacked && stack.get(other)) {
  return stacked == other
}
let result = true
stack.set(object, other)
stack.set(other, object)
...
stack['delete'](object)
stack['delete'](other)
```

循环依赖的逻辑和[《lodash源码分析之equalArrays》](./equalArrays.md)是一致的，这里不再细叙。

### 自定义比较函数比较

```javascript
while (++index < objLength) {
  key = objProps[index]
  const objValue = object[key]
  const othValue = other[key]

  if (customizer) {
    compared = isPartial
      ? customizer(othValue, objValue, key, other, object, stack)
    : customizer(objValue, othValue, key, object, other, stack)
  }
  ...
}
```

属性比较完后，就要比较每个属性上的值了。

如果有自定义比较函数 `customizer` ，则直接调用 `customizer` 比较即可，要注意的是，如果开启局部比较，`othValue` 和 `objValue` 的位置是互调的。

比较的结果存在 `compared` 变量中。

### 内部比较逻辑

```javascript
while (++index < objLength) {
  key = objProps[index]
  ...
  if (!(compared === undefined
        ? (objValue === othValue || equalFunc(objValue, othValue, bitmask, customizer, stack))
        : compared
       )) {
    result = false
    break
  }
  skipCtor || (skipCtor = key == 'constructor')
}
```

如果没有调用比较函数，则 `compared` 为 `undefined` ，或者有调用自定义比较函数，返回的结果是 `undefined` ，都使用内部的比较逻辑进行比较。

内部会首先使用 `===` 进行比较，如果得到的结果为不相等，则会使用 `equalFunc` 得到比较结果。

如果得到的结果为不相等，会立即跳出循环，返回的结果为 `false`。

如果得到的结果为 `true` ，并且在遍历的过程中，遇到 `key` 为 `constructor` ，即表示自身的属性有 `constructor`，这时 `constructor` 会作为正常的属性比较，会跳过后面的构造函数比较。

### 构造函数比较

```javascript
if (result && !skipCtor) {
  const objCtor = object.constructor
  const othCtor = other.constructor

  if (objCtor != othCtor &&
      ('constructor' in object && 'constructor' in other) &&
      !(typeof objCtor === 'function' && objCtor instanceof objCtor &&
        typeof othCtor === 'function' && othCtor instanceof othCtor)) {
    result = false
  }
}
```

如果自身属性上没有 `constructor`，则会进一步比较构造函数，如果构造函数相同，则表示这两个是同一个类的实例，属性在上面的比较中是相等的，因此也认为是相等的。

但是构造函数不相等时，有两种情况，也认为这两个对象是相等的。

#### Object.create(null)

如：

```javascript
var object1 = Object.create(null);
object1.a = 1

var object2 = { 'a': 1 }

```

此时 `object1` 的 `constructor` 为 `undefined` ，`object2` 的构造函数为 `Object` ，但是 `lodash` 认为这两种对象都是由 `Object` 创建的，也是相等的。

这便是第二条判断的作用：

```javascript
('constructor' in object && 'constructor' in other)
```

在这种情况下，这条判断返回的结果为 `false` 。

#### 跨域对象

如果不同的 `iframe` 中，构造函数都为 `Object` 创建了两个对象，此时，这两个对象的 `constructor` 是不相等的，因为隔离的环境的 `Object` 构造函数是不相等的。

但是 `Object` 有个很神奇的特性，`Object.constructor instanceOf Object.constructor` 为 `true` 。

因此可以利用这个特性，知道这两个对象是否都是由各自的 `Object` 构造函数直接创建的，如果是，则认为他们相等。

这也是第三个判断的作用：

```javascript
!(typeof objCtor === 'function' && objCtor instanceof objCtor &&
 typeof othCtor === 'function' && othCtor instanceof othCtor)
```


## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 