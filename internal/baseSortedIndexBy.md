# lodash源码分析之baseSortedIndexBy

本文为读 lodash 源码的第八十篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import isSymbol from '../isSymbol.js'
```

[《lodash源码分析之isSymbol》](../isSymbol.md)

## 源码分析

`baseSortedIndexBy` 是实现 `sortedIndexBy` 和 `sortedLastIndexBy` 的基础方法。

其作用是找出，指定的 `value` 在一个已经排好序的数组 `array` 中，应该插入的位置，这样就算这个 `value` 插入到这个位置，原来的数组的排序不会打乱。

因为同时要支持 `baseSortedIndexBy` 和 `sortedLastIndexBy`，也就是要支持插在前面和插在后面两种情况，`baseSortedIndexBy` 比这两个函数需要多一个参数来做标识，这个参数就是 `retHighest`。

具体源码如下：

```javascript
const MAX_ARRAY_LENGTH = 4294967295
const MAX_ARRAY_INDEX = MAX_ARRAY_LENGTH - 1

function baseSortedIndexBy(array, value, iteratee, retHighest) {
  let low = 0
  let high = array == null ? 0 : array.length
  if (high == 0) {
    return 0
  }

  value = iteratee(value)

  const valIsNaN = value !== value
  const valIsNull = value === null
  const valIsSymbol = isSymbol(value)
  const valIsUndefined = value === undefined

  while (low < high) {
    let setLow
    const mid = Math.floor((low + high) / 2)
    const computed = iteratee(array[mid])
    const othIsDefined = computed !== undefined
    const othIsNull = computed === null
    const othIsReflexive = computed === computed
    const othIsSymbol = isSymbol(computed)

    if (valIsNaN) {
      setLow = retHighest || othIsReflexive
    } else if (valIsUndefined) {
      setLow = othIsReflexive && (retHighest || othIsDefined)
    } else if (valIsNull) {
      setLow = othIsReflexive && othIsDefined && (retHighest || !othIsNull)
    } else if (valIsSymbol) {
      setLow = othIsReflexive && othIsDefined && !othIsNull && (retHighest || !othIsSymbol)
    } else if (othIsNull || othIsSymbol) {
      setLow = false
    } else {
      setLow = retHighest ? (computed <= value) : (computed < value)
    }
    if (setLow) {
      low = mid + 1
    } else {
      high = mid
    }
  }
  return Math.min(high, MAX_ARRAY_INDEX)
}
```

### 数组长度最大值

数组长度最大值 `MAX_ARRAY_LENGTH` 为 `4294967295` ，因为 `value` 需要插入，因此最大安全的插入位置为 `MAX_ARRAY_LENGTH - 1` 处。

### 处理边界情况

```javascript
let low = 0
let high = array == null ? 0 : array.length
if (high == 0) {
  return 0
}
```

这段处理的是数组没传，或者数组传入为空的情况，这时，直接返回 `0` 。

接下来就是使用二分查找的方式，来确定 `value` 最终要插入的位置。

### value 取值

```javascript
value = iteratee(value)
```

在比较的时候，可能并不是使用传入的 `value` 来比较的，需要调用 `iteratee` 迭代器来取得 `value` 的计算值，在二分查找的时候，也会调用 `iteratee` 来计算比较的值。

```javascript
const computed = iteratee(array[mid])
```

因为数组中包含的可能不一定为数字，例如传入一个对象 `{name: 'test', number: 1}` ，其实 `number` 是排序数组的主键，调用 `iteratee` 时，应该返回数字 `1` 来进行比较。

### 处理value为NaN的情况

`setLow` 表示排除低位，每次二分后，如果 `setLow` 为 `true` ，表示结果需要在高位的那半数组中查找。

```javascript
if (valIsNaN) {
  setLow = retHighest || othIsReflexive
}
```

如果 `value` 传入的为 `NaN` 值，如果 `retHighest` 传入的为 `true` ，则 `setLow` 也为 `true` ，如果在当前二分的过程中，没有遇到 `NaN` 值，即 `othIsReflexive`，`setLow` 也为 `true`，即将低位数组舍弃，继续向高位找 `value` 的位置。

`retHighest` 很容易理解，例如数组 `[1,2,3,4]` ，如果传入 `retHightest` 为 `true`，`value` 为 `NaN`，则一直向高位查找，因为每次二分时，低位都不符合，最后返回的是高位 `high`，这个值为 `array.length`，也即是 `4` ，表示 `value` 。

但是对于 `othIsReflexive` 这个条件我是有疑惑的，因为如果是这样的数组 `[NaN, 1, 2, 3]` ，`retHightest` 传入的是 `false` ，但是最终的结果返回的会是 `4` ，因为在第一次二分时，`computed` 的值为 `2` ，也即 `othIsReflexive` 为 `true` ，直接将低位舍弃了，继续向高位查找时，`othIsReflexive` 也都为 `true` ，直到数组查找完毕，最后返回的也就是 `4` 了，但是我本来期望它返回 `0` 才对。

### 处理undefined的情况

```javascript
if (valIsUndefined) {
  setLow = othIsReflexive && (retHighest || othIsDefined)
}
```

这个逻辑其实和 `NaN` 的处理有点类似，`othIsReflexive` 表示当前二分位的值不为 `NaN`，如果有 `NaN` 值，则舍弃高位，向低位查找插入的位置。

然后判断 `retHighest` 是否为 `true` ，如果是，则向高位查找；如果当前二分位的值有定义，也继续向高位查找。

### 处理null的情况

```javascript
if (valIsNull) {
  setLow = othIsReflexive && othIsDefined && (retHighest || !othIsNull)
}
```

处理 `null` 的情况和处理 `undefined` 的情况是类似的。

在处理 `null` 时，如果当前二分位的值为 `NaN` 或者为 `undefined`，则向低位查找。

如果都不是，则判断 `retHighest` 是否为 `true`，如果为 `true`，直接向高位查找；如果当前二分位的值不为 `null`，也是继续向高位查找。

### 处理symbol的情况

```javascript
setLow = othIsReflexive && othIsDefined && !othIsNull && (retHighest || !othIsSymbol)
```

为什么要处理 `symbol` 呢，因为 `symbol` 是不能隐式转换成 `number` ，也即没办法比较大小。

`symbol` 的处理和前面几种类型的逻辑是差不多的，只不过在排除低位时，将 `NaN`，`undefined` 和 `null` 都考虑上了。

这样的目的也很明显，因为 `symbol` 毕竟是已经定义了的，不应该排在 `NaN`，`undefined` 或者 `null` 的前面。

### 处理二分中位值的特殊情况

```javascript
if (othIsNull || othIsSymbol) {
  setLow = false
}
```

在考虑完 `value` 的以上的情况后，此时的 `value` 是不为 `NaN`、`undefined`、`null` 和 `symbol`，此时如果当前二分中位值为 `null` 或者 `symbol`，则向低位数组查找 `value` 的位置。

这块有点不是很明白，如果逻辑一致，中位值为 `NaN` 或者为 `undefined` 时，也应该向下查找才对。

### 直接比较大小的情况

```javascript
setLow = retHighest ? (computed <= value) : (computed < value)
```

处理完特殊情况后，就直接用中位值 `computed` 和 `value` 进行比较，如果中位值比 `value` 小，表示 `value` 的位置应该在高位。

如果 `retHighest` 有值，则在相等的情况下，也向高位继续找 `value` 的位置。

例如 `[1,1,1,2]` ，传入的 `value` 为 `1`，在 `retHighest` 为 `true` 的情况下，返回的 `index` 为 `3`，否则返回的 `index` 为 `0` 。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 