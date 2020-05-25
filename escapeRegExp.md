# lodash源码分析之escapeRegExp

本文为读 lodash 源码的第三百二十一篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`escapeRegExp` 会对在正则表达式中有特殊含义的字符进行转义。

即如下这些字符：

```javascript
"^", "$", "\", ".", "*", "+", "?", "(", ")", "[", "]", "{", "}", "|" 
```

源码如下：

```javascript
const reRegExpChar = /[\\^$.*+?()[\]{}|]/g
const reHasRegExpChar = RegExp(reRegExpChar.source)

function escapeRegExp(string) {
  return (string && reHasRegExpChar.test(string))
    ? string.replace(reRegExpChar, '\\$&')
    : (string || '')
}
```

`reHasRegExpChar` 用来检测字符串中是否有以上列举的字符的正则。

调用正则的 `test` 方法来检测字符串中是否包含这些字符，如果有，则调用字符串的 `replace` 方法，将匹配到的字符在前面加上一个 `\` 来转义。

可以看到，`replace` 方法用了 `$&` 来获取被匹配到的字符串，这是 `replace` 的特殊变量名。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

