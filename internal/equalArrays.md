# lodash源码分析之equalArrays

本文为读 lodash 源码的第二百一十四篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import SetCache from './SetCache.js'
import some from '../some.js'
import cacheHas from './cacheHas.js'
```

[《lodash源码分析之SetCache》](./SetCache.md)
[《lodash源码分析之some》](../some.md)
[《lodash源码分析之cacheHas》](./cacheHas.md)

## 源码分析

`equalArrays` 用来比较两个数组是否相等，这个相等不只是使用 `===` 来进行比较，也即不仅仅是指内存地址引用的相同，而是会深度比较，再来判断是否相等。

### 参数

* *array*： 需要比较的数组
* *other*: 另外一个需要比较的数组
* *bitmask*： 标志位，可以用来控制部分比较和无序数组的比较
* *customizer*： 自定义比较函数
* *equalFunc*：比较函数
* *stack*：`Stack` 实例，用来防止出现循环引用的情况

### 长度比较

```javascript
const isPartial = bitmask & COMPARE_PARTIAL_FLAG
const arrLength = array.length
const othLength = other.length

if (arrLength != othLength && !(isPartial && othLength > arrLength)) {
  return false
}
```

两个数组不相等，最简单的就是两个数组的长度不一样，那就没有相等的可能。

但是 `isPartial` 可以控制是否部分比较，部分比较要求 `otherLength` 的长度比 `arrLength` 的长度要长，也即只比较两者 `0 - arrLength` 部分的值。

### 循环引用的比较

```javascript
const stacked = stack.get(array)
if (stacked && stack.get(other)) {
  return stacked == other
}

...
stack.set(array, other)
stack.set(other, array)

... // 主要比较逻辑，可能会递归调用equalArrays

stack['delete'](array)
stack['delete'](other)
```

使用 `stack` 来保存 `array` 和 `other` ，可以看到，用 `array` 作为 `key` 时，值为 `other` 。

因此 `stack.get(array)` 取到的会是 `other` 。

例如以下的数组，就会出现循环引用的情况：

```javascript
const array = [1]
const other = [1]

array.push(array)
other.push(ohter)
```

在第一次进入 `equalArrays` 时，`stack.get(array)` 是不会有值的。此时用 `array` 作为 `key` 来存 `other` ，用 `other` 作为 `key` 存 `array` ，后面会看到，这一点很巧妙。

再进入 `equalArrays` 时，上一轮的比较肯定是相等的，此时从 `stack` 中取出 `array` ，如果有值，则 `array` 肯定进入循环引用了，再从 `stack` 中取出 `other` ，如果有值，表示 `other` 也进入循环引用了，然后用 `stacked` 即上一轮的 `other` 值和当前的 `other` 值比较，如果相等，则表示 `array` 和 `other` 是相等的，因为上一轮已经和 `array` 比较过是相等的了。

最后还要从 `stack` 中删除。

### 自定义比较函数

处理完循环引用的比较，接下来就遍历 `array` ，逐个元素来比较了。

```javascript
...
let index = -1
let result = true
const seen = (bitmask & COMPARE_UNORDERED_FLAG) ? new SetCache : undefined
...
while (++index < arrLength) {
  let compared
  const arrValue = array[index]
  const othValue = other[index]

  if (customizer) {
    compared = isPartial
      ? customizer(othValue, arrValue, index, other, array, stack)
    : customizer(arrValue, othValue, index, array, other, stack)
  }
  if (compared !== undefined) {
    if (compared) {
      continue
    }
    result = false
    break
  }
}
```

如果有传自定义比较函数，则直接使用自定义比较函数进行比较，在部分比较模式下，`othValue` 和 `arrValue` 的位置是互调的。

如果自定义比较函数返回的结果不是 `undefined` ，则使用自定义比较函数的结果，在假值的情况下直接跳出循环，得到的结果为 `false`。

在返回 `undefined` 的情况下，表示自定义比较函数希望使用内置的逻辑来进行比较。

### 无序比较

```javascript
if (seen) {
  if (!some(other, (othValue, othIndex) => {
    if (!cacheHas(seen, othIndex) &&
        (arrValue === othValue || equalFunc(arrValue, othValue, bitmask, customizer, stack))) {
      return seen.push(othIndex)
    }
  })) {
    result = false
    break
  }
}
```

如果 `seen` 存在，表示要进行无序比较。

上面的逻辑其实可以简化成：

```javascript
if (!some(other, (othValue) => arrValue === othValue)) {
  return false
}
```

其实就是判断每个 `array` 中每个值是否在都 `other` 中存在，如果都存在，则两个数组是相等的，否则就不相等

`seen` 是用来做缓存的，因此用 `some` 比较时，首先用 `cacheHas` 来判断当前值的索引是否存在于缓存中了，如果已经存在，则表示当前值已经比较过是相等的了，没必要再比较。

如果没有，没优先使用 `===` ，即全等的方式来比较，如果比较不出来，则再使用内部的比较函数 `equalFunc` 来比较，这个 `equalFuc` 即是 `equalArrays` 可能出现递归调用的原因。

如果相等，则将索引值存在入 `seen` 中，缓存起来。

### 有序比较

```javascript
else if (!(
  arrValue === othValue ||
  equalFunc(arrValue, othValue, bitmask, customizer, stack)
)) {
  result = false
  break
}
```

有序比较就简单了，每次 `while` 循环的时候，使用 `===` 比较或者 `equalFunc` 比较即可。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 