# lodash源码分析之uniqBy

本文为读 lodash 源码的第一百零三篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseUniq from './.internal/baseUniq.js'
```

[《lodash源码分析之baseUniq》](internal/baseUniq.md)

## 源码分析

和`uniq` 一样， `uniqBy` 也是简单地调用 `baseUniq` 来实现，只不过比 `uniq` 多传一个 `iteratee`  参数。

源码如下：

```javascript
function uniqBy(array, iteratee) {
  return (array != null && array.length)
    ? baseUniq(array, iteratee)
    : []
}
```

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 