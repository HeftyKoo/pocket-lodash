# lodash源码分析之clone

本文为读 lodash 源码的第一百九十九篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseClone from './.internal/baseClone.js'
```

[《lodash源码分析之baseClone》](internal/baseClone.md)


## 源码分析

`clone` 会对传入的 `value` 做浅复制，源码如下：

```javascript
const CLONE_SYMBOLS_FLAG = 4
function clone(value) {
  return baseClone(value, CLONE_SYMBOLS_FLAG)
}
```

可以看到，其实调用的是 `baseClone` ，并且 `bitmask` 传入的是 `CLONE_SYMBOLS_FLAG`，即复制 `Symbol` 类型的属性。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 