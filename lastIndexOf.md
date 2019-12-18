# lodash源码分析之lastIndexOf

本文为读 lodash 源码的第五十五篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseFindIndex from './.internal/baseFindIndex.js'
import baseIsNaN from './.internal/baseIsNaN.js'
import strictLastIndexOf from './.internal/strictLastIndexOf.js'
import toInteger from './toInteger.js'
```

[《lodash源码分析之baseFindIndex中的运算符优先级》](internal/baseFindIndex.md)

[《lodash源码分析之NaN不是NaN》](eq.md)

[《lodash源码分析之strictLastIndexOf》](internal/strictLastIndexOf.md)

[《lodash源码分析之toInteger》](toInteger.md)

## 源码分析

`lastIndexOf` 的作用是从后往前查找指定 `value` 值在数组 `array` 中的索引值，同时可以指定 `fromIndex` ，指定查找的开始位置。

源码如下：

```javascript
function lastIndexOf(array, value, fromIndex) {
  const length = array == null ? 0 : array.length
  if (!length) {
    return -1
  }
  let index = length
  if (fromIndex !== undefined) {
    index = toInteger(fromIndex)
    index = index < 0 ? Math.max(length + index, 0) : Math.min(index, length - 1)
  }
  return value === value
    ? strictLastIndexOf(array, value, index)
    : baseFindIndex(array, baseIsNaN, index, true)
}
```

### 空值判断

```javascript
const length = array == null ? 0 : array.length
if (!length) {
  return -1
}
```

如果 `array` 没有传，或者传入的 `array` 为空，则直接返回 `-1`

### 指定开始查找的位置

```javascript
let index = length
if (fromIndex !== undefined) {
  index = toInteger(fromIndex)
  index = index < 0 ? Math.max(length + index, 0) : Math.min(index, length - 1)
}
```

先默认从数组的最后位置开始查起，即 `index = length`

如果有指定 `fromIndex`，则先将 `fromIndex` 合规化为整数，并默认从指定的位置开始查起。

此时，如果 `index < 0` ，则取 `length + index` 和 `0` 的最大值，避免出现负值。如 `length` 为 `10` 的数组，如果指定了 `fromIndex` 为 `-2` ，则从索引值为 `8` 的地方处开始查起，如果指定了 `fromIndex` 为 `-12`，则从索引 `0` 处查起，最终也会返回 `-1`。

如果 `index` 不小于 `0` ，则取 `index` 和 `length-1` 的最小值，避免超出数组的最大索引。

### 处理NaN值

```javascript
value === value
    ? strictLastIndexOf(array, value, index)
    : baseFindIndex(array, baseIsNaN, index, true)
```

如果 `value === value` ，则表示要查找的 `value` 不为 `NaN` 值，直接调用 `strictLastIndexOf` 即可。

如果为 `NaN` 值，则使用 `baseFindeIndex`，并且使用 `baseIsNaN` 作为比较函数。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 