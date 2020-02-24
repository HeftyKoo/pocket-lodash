# lodash源码分析之sortedLastIndexOf

本文为读 lodash 源码的第八十七篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseSortedIndex from './.internal/baseSortedIndex.js'
import eq from './eq.js'
```

[《lodash源码分析之baseSortedIndex》](./internal/baseSortedIndex.md)

[《lodash源码分析之NaN不是NaN》](eq.md)

## 源码分析

`sortedLastIndexOf` 的作用和 `sortedIndexOf` 的作用差不多，但是 `sortedIndexOf` 返回的是已排好序的数组中，第一个 `value` 值的索引，`srotedLastIndexOf` 返回的是最后一个 `value` 值的索引。

例如 `[1,2,2,3]` 这个数组，`value` 指定为 `2`，使用 `sortedIndexOf` 返回的是 `1`，而使用 `sortedLastIndexOf` 返回的是 `2` 。

源码如下：

```javascript
function sortedLastIndexOf(array, value) {
  const length = array == null ? 0 : array.length
  if (length) {
    const index = baseSortedIndex(array, value, true) - 1
    if (eq(array[index], value)) {
      return index
    }
  }
  return -1
}
```

源码基本和 `sortedIndexOf` 一致，只不过在调用 `baseSortedIndex` 的时候会传入第三个参数为 ` retHighest` 为 `true`。

因为 `retHighest` 为 `true`，指的是 `value` 要插在后面一位，因为使用 `baseSortedIndex` 获取的结果要减掉 `1` 才是 `value` 可能存在的位置。

然后使用 `eq` 来判断这个位置上的值和传入的 `value` 是否相等。

注意到，这里没有 `index < length` 的判断。因为 `baseSortedIndex` 返回的最大值也就是 `length` ，现在 `length - 1` 刚好是最后的索引值，因此，无论怎样，`index` 都不会超过数组的最大索引，所以没有必要再判断一次。

`sortedIndexOf`的分析见：[lodash源码分析之sortedLastIndexOf](sortedIndexOf.md)

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 