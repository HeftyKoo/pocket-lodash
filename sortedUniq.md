# lodash源码分析之sortedUniq

本文为读 lodash 源码的第八十九篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseSortedUniq from './.internal/baseSortedUniq.js'
```

[《lodash源码分析之baseSortedUniq》](internal/baseSortedUniq.md)

## 源码分析

`sortedUniq` 调用的其实就是 `baseSortedUniq` 函数。

源码如下：

```javascript
function sortedUniq(array) {
  return (array != null && array.length)
    ? baseSortedUniq(array)
    : []
}
```

如果数组没有传入，或者传入的为空数组，则直接返回空数组。

否则调用 `baseSortedUniq` 函数来去重。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 