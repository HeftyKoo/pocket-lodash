# lodash源码分析之baseSortedIndex

本文为读 lodash 源码的第八十一篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseSortedIndexBy from './baseSortedIndexBy.js'
import isSymbol from '../isSymbol.js'
```

[《lodash源码分析之baseSortedIndexBy》](./baseSortedIndexBy.md)

[《lodash源码分析之isSymbol》](../isSymbol.md)

## 源码分析

`baseSortedIndex` 的作用和 `baseSortedIndexBy` 的作用差不多，但是比 `baseSortedIndexBy` 少一个 `iteratee` 参数，也即 `baseSortedIndex` 在二分查找的过程中，直接使用传入的 `value` 和中位值进行比较，不会再调用 `iteratee` ，再用返回的值进行比较。

源码如下：

```javascript
const MAX_ARRAY_LENGTH = 4294967295
const HALF_MAX_ARRAY_LENGTH = MAX_ARRAY_LENGTH >>> 1

function baseSortedIndex(array, value, retHighest) {
  let low = 0
  let high = array == null ? low : array.length

  if (typeof value === 'number' && value === value && high <= HALF_MAX_ARRAY_LENGTH) {
    while (low < high) {
      const mid = (low + high) >>> 1
      const computed = array[mid]
      if (computed !== null && !isSymbol(computed) &&
          (retHighest ? (computed <= value) : (computed < value))) {
        low = mid + 1
      } else {
        high = mid
      }
    }
    return high
  }
  return baseSortedIndexBy(array, value, (value) => value, retHighest)
}
```

其实，`baseSortedIndexBy` 完全满足了 `baseSortedIndex` 的需求，只需要最后一段即可。

```javascript
function baseSortedIndex(array, value, retHighest) {
  return baseSortedIndexBy(array, value, (value) => value, retHighest)
}
```

但是，`baseSortedIndexBy` 处理了太多 `value` 为特殊值的问题，而且每次都需要调用 `iteratee` ，性能会有一定影响。

因此判断 `value` 为 `number` 类型，并且 `value` 不为 `NaN`，也即 `value === value` 时，还有数组长度小于 `HALF_MAX_ARRAY_LENGTH` 时，直接走 `baseSortedIndexBy` 最后两个分支：

```javascript
if (othIsNull || othIsSymbol) {
  setLow = false
} else {
  setLow = retHighest ? (computed <= value) : (computed < value)
}
if (setLow) {
  low = mid + 1
} else {
  high = mid
}
```

相当于 `baseSortedIndex` 中的这块代码：

```javascript
if (computed !== null && !isSymbol(computed) &&
    (retHighest ? (computed <= value) : (computed < value))) {
  low = mid + 1
} else {
  high = mid
}
```

至少为什么数组长度要小于最大数组长度的一半呢，我也不是很清楚。

在源码中还有一点非常有趣：

```javascript
const mid = (low + high) >>> 1
```

这里使用无符号右移来计算 `mid` 值。

这个其实相当于：

```javascript
Math.floor((low + high) / 2)
```

但是无符号位右移性能比 `Math.floor` 好一点点，基于没有差别，在平时的代码中，建议用 `Math.floor` 的方式，更加易于理解。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 