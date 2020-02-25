# lodash源码分析之sortedUniqBy

本文为读 lodash 源码的第九十篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseSortedUniq from './.internal/baseSortedUniq.js'
```

[《lodash源码分析之baseSortedUniq》](internal/baseSortedUniq.md)

## 源码分析

和 `sortedUniq` 一样， `sortedUniqBy` 调用的也是 `baseSortedUniq` 函数。

源码如下：

```javascript
function sortedUniqBy(array, iteratee) {
  return (array != null && array.length)
    ? baseSortedUniq(array, iteratee)
    : []
}
```

`sortedUniqBy` 可以传入 `iteratee` 函数，对数组中每一项进行转换再进行比较，不像 `sortedUniq` 一样，直接使用原始值比较。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 