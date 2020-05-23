# lodash源码分析之words

本文为读 lodash 源码的第三百一十三篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import unicodeWords from './.internal/unicodeWords.js'
```

[《lodash源码分析之unicodeWords》](./internal/unicodeWords.md)

## 源码分析

`words` 的作用是将字符串分割成单词数组。

源码如下：

```javascript
const hasUnicodeWord = RegExp.prototype.test.bind(
  /[a-z][A-Z]|[A-Z]{2}[a-z]|[0-9][a-zA-Z]|[a-zA-Z][0-9]|[^a-zA-Z0-9 ]/
)
const reAsciiWord = /[^\x00-\x2f\x3a-\x40\x5b-\x60\x7b-\x7f]+/g
function asciiWords(string) {
  return string.match(reAsciiWord)
}
function words(string, pattern) {
  if (pattern === undefined) {
    const result = hasUnicodeWord(string) ? unicodeWords(string) : asciiWords(string)
    return result || []
  }
  return string.match(pattern) || []
}
```

`hasUnicodeWord` 用来判断字符串中是否有 `unicode` 字符。

`reAsciiWord` 用来匹配 `ascii` 单词，这个正则其实是排除 `ascii` 中的特殊字符。可以参考维基上的码表 [https://zh.wikipedia.org/wiki/ASCII](https://zh.wikipedia.org/wiki/ASCII)

函数 `asciiWords` 就是利用正则来匹配出以特殊字符分隔的单词，用 `match` 方法，最后得到的就是单词数组。

`words` 支持传入匹配正则，如果有传入，就直接使用传入的正则来匹配。

如果没有传入，则先判断字符串是否有 `unicode` 编码，如果有，则调用 `unicodeWords` 来匹配，否则使用 `asciiWords` 方法来匹配。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

