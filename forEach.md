# lodash源码分析之forEach

本文为读 lodash 源码的第一百三十七篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import arrayEach from './.internal/arrayEach.js'
import baseEach from './.internal/baseEach.js'
```

[《lodash源码分析之arrayEach》](internal/arrayEach.md)
[《lodash源码分析之baseEach》](internal/baseEach.md)

## 源码分析

`forEach` 的作用和 `array` 的 `forEach` 方法类似，不过 `forEach` 方法除了支持数组外，也支持对象的迭代。

源码如下：

```javascript
function forEach(collection, iteratee) {
  const func = Array.isArray(collection) ? arrayEach : baseEach
  return func(collection, iteratee)
}
```

如果 `collection` 为数组，则使用 `arrayEach` 方法，否则使用 `baseEach` 方法。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 