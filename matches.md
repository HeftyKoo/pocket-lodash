# lodash源码分析之matches

本文为读 lodash 源码的第三百五十三篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseClone from './.internal/baseClone.js'
import baseMatches from './.internal/baseMatches.js'
```

[《lodash源码分析之baseClone》](./internal/baseClone.md)

[《lodash源码分析之baseMatches》](./internal/baseMatches.md)


## 源码分析

`matches` 其实是 `isMatch` 的偏函数，接收 `source` 对应，返回一个函数，返回的函数接收 `object` 对象，用来检测 `source` 对象上所有的属性值是否和 `object` 上的相等。

源码如下：

```javascript
const CLONE_DEEP_FLAG = 1
function matches(source) {
  return baseMatches(baseClone(source, CLONE_DEEP_FLAG))
}
```

其实就是调用 `baseMatches` ，只不过将 `source` 传入时，会进行深度复制。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

