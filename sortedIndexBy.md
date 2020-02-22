# lodash源码分析之sortedIndexBy

本文为读 lodash 源码的第八十三篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseSortedIndexBy from './.internal/baseSortedIndexBy.js'
```

[《lodash源码分析之baseSortedIndexBy》](./internal/baseSortedIndexBy.md)

## 源码分析

`sortedIndexBy` 其实就是调用了 `baseSortedIndexBy` ：

```javascript
function sortedIndex(array, value, iteratee) {
  return baseSortedIndex(array, value, iteratee)
}
```

没有传第四个参数 `retHighest` ，也即这个参数默认为 `false` 。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 