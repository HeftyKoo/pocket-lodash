# lodash源码分析之get

本文为读 lodash 源码的第七十二篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseGet from './.internal/baseGet.js'
```

[《lodash源码分析之baseGet》](./internal/baseGet.md)

## 源码分析

`get` 函数可以对对象或者数组深层取值，主要功能是由 `baseGet` 函数来实现，但是 `get` 函数可以设置默认值。

```javascript
function get(object, path, defaultValue) {
  const result = object == null ? undefined : baseGet(object, path)
  return result === undefined ? defaultValue : result
}
```

首先判断 `object` 是否存在，如果不存在则 `result` 直接为 `undefined`，否则使用 `baseGet` 取值。

接着判断返回的 `result` 是否为 `undefined`，如果为 `undefined`，则返回默认值 `defaultValue`，否则返回取值结果 `result`。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 