# lodash源码分析之cloneDeepWith

本文为读 lodash 源码的第二百零一篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseClone from './.internal/baseClone.js'
```

[《lodash源码分析之baseClone》](internal/baseClone.md)


## 源码分析

`cloneDeepWith` 可以自定义复制函数：

```javascript
const CLONE_DEEP_FLAG = 1
const CLONE_SYMBOLS_FLAG = 4

function cloneDeepWith(value, customizer) {
  customizer = typeof customizer === 'function' ? customizer : undefined
  return baseClone(value, CLONE_DEEP_FLAG | CLONE_SYMBOLS_FLAG, customizer)
}
```

`cloneDeepWith` 支持深度复制、支持复制 `Symbol` 属性，同时也支持传入 `customizer` 自定义复制函数。

最后其实也是调用 `baseClone` ，在调用 `baseClone` 之前会对 `customizer` 做一个是否为函数的检测。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 