# lodash源码分析之cloneWith

本文为读 lodash 源码的第二百零二篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseClone from './.internal/baseClone.js'
```

[《lodash源码分析之baseClone》](internal/baseClone.md)


## 源码分析

和 `cloneDeepWith` 一样，`cloneWith` 也支持自定义复制函数：

```javascript
const CLONE_SYMBOLS_FLAG = 4
function cloneWith(value, customizer) {
  customizer = typeof customizer === 'function' ? customizer : undefined
  return baseClone(value, CLONE_SYMBOLS_FLAG, customizer)
}
```

也是调用 `baseClone` 来实现，在调用之前对 `customizer` 做是否为函数的检测。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 