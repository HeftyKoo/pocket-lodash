# lodash源码分析之forEachRight

本文为读 lodash 源码的第一百四十二篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import arrayEachRight from './.internal/arrayEachRight.js'
import baseEachRight from './.internal/baseEachRight.js'
```

[《lodash源码分析之arrayEachRight》](internal/arrayEachRight.md)
[《lodash源码分析之baseEachRight》](internal/baseEachRight.md)

## 源码分析

`forEachRight` 和 `forEach` 的方法类似，不过是从后向前遍历。

源码如下：

```javascript
function forEachRight(collection, iteratee) {
  const func = Array.isArray(collection) ? arrayEachRight : baseEachRight
  return func(collection, iteratee)
}
```

和 `forEach` 不同的是，`forEachRight` 调用的是 `arrayEachRight` 或者 `baseEachRight`。

`forEach` 的源码分析：[《lodash源码分析之forEach》](./forEach.md)

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 