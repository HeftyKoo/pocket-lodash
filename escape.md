# lodash源码分析之escape

本文为读 lodash 源码的第三百二十篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`escape` 会对 `&` 、`<` 、`>` 、`"` 和 `'` 这些 `HTML` 实体进行编码。

源码如下：

```javascript
const htmlEscapes = {
  '&': '&amp;',
  '<': '&lt;',
  '>': '&gt;',
  '"': '&quot;',
  "'": '&#39;'
}
const reUnescapedHtml = /[&<>"']/g
const reHasUnescapedHtml = RegExp(reUnescapedHtml.source)

function escape(string) {
  return (string && reHasUnescapedHtml.test(string))
    ? string.replace(reUnescapedHtml, (chr) => htmlEscapes[chr])
    : (string || '')
}
```

`htmlEscapes` 用来建立 `HTML` 实体和编码之间的映射。

`reHasUnescapedHtml` 用来检测字符串中是否有未编码实体的正则。

所以只需要用 `reHasUnescapedHtml` 正则来检测字符串，如果含有未编码的 `HTML` 实体，则调用字符串的 `replace` 方法，将字符串中的实体替换成编码。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

