# lodash源码分析之unescape

本文为读 lodash 源码的第三百四十二篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`unescape` 的作用和 `escape` 刚好相反，是将编码转换成 `HTML` 实体。

源码如下：

```javascript
const htmlUnescapes = {
  '&amp;': '&',
  '&lt;': '<',
  '&gt;': '>',
  '&quot;': '"',
  '&#39;': "'"
}

const reEscapedHtml = /&(?:amp|lt|gt|quot|#39);/g
const reHasEscapedHtml = RegExp(reEscapedHtml.source)

function unescape(string) {
  return (string && reHasEscapedHtml.test(string))
    ? string.replace(reEscapedHtml, (entity) => htmlUnescapes[entity])
    : (string || '')
}
```

可以看到，逻辑基本和 `escape` 一致，只在映射表和匹配的正则上有差异。

`unescape` 的映射表和 `escape` 刚好相反，用编码映射实体，正则匹配的是编码而不是实体。

`escape` 的分析见：[《lodash源码分析之escape》](./escape.md)

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

