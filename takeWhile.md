# lodash源码分析之takeWhile

本文为读 lodash 源码的第九十五篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseWhile from './.internal/baseWhile.js'
```

[《lodash源码分析之baseWhile》](internal/baseWhile.md)

## 源码分析

源码如下：

```javascript
function takeWhile(array, predicate) {
  return (array != null && array.length)
    ? baseWhile(array, predicate)
    : []
}
```

`takeWhile` 和 `takeRightWhile` 的源码基本一致，只是在调用 `baseWhile` 的时候，第四个参数没有传递，也即 `fromRight` 为 `false` ，表示从前往后取值，取值顺序想反。

`takeRightWhile` 源码分析见：[《lodash源码分析之takeRightWhile》](takeRightWhile.md)

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 