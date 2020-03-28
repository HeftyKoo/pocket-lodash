# lodash源码分析之stringSize

本文为读 lodash 源码的第一百六十九篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import asciiSize from './asciiSize.js'
import hasUnicode from './hasUnicode.js'
import unicodeSize from './unicodeSize.js'
```

[《lodash源码分析之asciiSize》](./asciiSize.md)
[《lodash源码分析之hasUnicode》](./hasUnicode.md)
[《lodash源码分析之unicodeSize》](./unicodeSize.md)

## 源码分析

`stringSize` 用来计算 `string` 的长度，一般情况下用字符串的 `length` 属性即可计算长度，但是 `unicode` 编码有点特殊。

源码如下：

```javascript
function stringSize(string) {
  return hasUnicode(string) ? unicodeSize(string) : asciiSize(string)
}
```

如果 `string` 中有 `unicode` 字符，则使用 `unicodeSize` 计算长度，否则使用 `asciiSize` 来计算长度。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 