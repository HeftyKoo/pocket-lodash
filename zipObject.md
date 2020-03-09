# lodash源码分析之zipObject

本文为读 lodash 源码的第一百一十八篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import assignValue from './.internal/assignValue.js'
import baseZipObject from './.internal/baseZipObject.js'
```

[《lodash源码分析之assignValue》](internal/assignValue.md)
[《lodash源码分析之baseZipObject》](internal/baseZipObject.md)

## 源码分析

`zipObject`  其实是 `baseZipObject` 的调用，只不过做了一些参数默认值的工作，并且将 `assignFunc` 设定为 `assignValue` 。

源码如下：

```javascript
function zipObject(props, values) {
  return baseZipObject(props || [], values || [], assignValue)
}
```

如果 `props` 没传，会默认为空数组，`values` 也同理。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 