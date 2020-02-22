# lodash源码分析之sortedIndexOf

本文为读 lodash 源码的第八十四篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseSortedIndex from './.internal/baseSortedIndex.js'
import eq from './eq.js'
```

[《lodash源码分析之baseSortedIndex》](./internal/baseSortedIndex.md)

[《lodash源码分析之NaN不是NaN》](eq.md)

## 源码分析

`sortedIndexOf` 的作用跟 `indexOf` 差不多，都是查找指定的 `value` 在数组 `array` 中的位置。

但是 `sortedIndexOf` 要求传入的数组是已经排序好的数组，在这种情况下，因为 `sortedIndexOf` 会使用二分法查找，比 `indexOf` 的性能更加高。

具体源码如下：

```javascript
function sortedIndexOf(array, value) {
  const length = array == null ? 0 : array.length
  if (length) {
    const index = baseSortedIndex(array, value)
    if (index < length && eq(array[index], value)) {
      return index
    }
  }
  return -1
}
```

首先获取数组 `array` 的长度，如果数组没有传，或者数组的长度为 `0` ，也即空数组，直接返回 `-1` ，跟 `indexOf` 的行为一致。

接着调用 `baseSortedIndex`，由 `baseSortedIndex` 的源码分析可知，`baseSortedIndex` 使用的是二分法查找，这个找出的是 `value` 应该插入到数组中的位置`index` 。

这时，还不能断定 `value` 在数组中存在，要判断这个 `index` 为 `value` 在数组中的位置，首先，`index` 要比数组的长度小，否则，现在的数组根本没有这个 `index`。

其次，在 `index` 位置上的值要和 `value` 相等，这里使用 `eq` 函数进行判断。

如果满足这两个条件，返回 `value` 所在的索引值 `index`，否则返回 `-1` 。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 