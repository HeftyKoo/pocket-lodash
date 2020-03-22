# lodash源码分析之baseForOwnRight

本文为读 lodash 源码的第一百四十篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseForRight from './baseForRight.js'
import keys from '../keys.js'
```

[《lodash源码分析之baseForRight》](./baseForRight.md)
[《lodash源码分析之keys》](../keys.md)

## 源码分析

`baseForOwnRight` 和 `baseForOwn` 的方法类似，不过是从后向前遍历，源码也大致一样。

源码如下：

```javascript
function baseForOwnRight(object, iteratee) {
  return object && baseForRight(object, iteratee, keys)
}
```

其实就是直接调用 `baseForRight` 传入对应的参数，`baseForOwn` 调用的是 `baseFor` 。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 