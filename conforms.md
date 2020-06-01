# lodash源码分析之conforms

本文为读 lodash 源码的第三百四十七篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseClone from './.internal/baseClone.js'
import baseConforms from './.internal/baseConforms.js'
```

[《lodash源码分析之baseClone》](./internal/baseClone.md)

[《lodash源码分析之baseConforms》](./internal/baseConforms.md)

## 源码分析

`conforms` 的作用是接收一个 `source` 对象，并返回一个函数，这个函数接收一个对象 `object` ，在调用这个函数的时候，会触发 `source` 上对应属性的断言函数，如果所有断言函数返回的结果都为真值，则得到的结果是 `true` ，否则得到的结果是 `false` 。

源码如下：

```javascript
const CLONE_DEEP_FLAG = 1
function conforms(source) {
  return baseConforms(baseClone(source, CLONE_DEEP_FLAG))
}
```

其实 `conforms` 本质上是 `comformsTo` 的偏函数。

使用 `baseClone` 来深度复制 `source` ，传入 `baseConforms` ，`baseConforms` 会创建一个函数，这个函数会调用 `baseConformsTo` ，逻辑就和 `conformsTo` 一致了。

具体分析见：[《lodash源码分析之conforms》](./conformsTo.md)

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

