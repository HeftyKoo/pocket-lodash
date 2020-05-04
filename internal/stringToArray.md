# lodash源码分析之stringToArray

本文为读 lodash 源码的第二百四十四篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import asciiToArray from './asciiToArray.js'
import hasUnicode from './hasUnicode.js'
import unicodeToArray from './unicodeToArray.js'
```

[《lodash源码分析之asciiToArray》](./asciiToArray.md)

[《lodash源码分析之hasUnicode》](./hasUnicode.md)

[《lodash源码分析之unicodeToArray》](./unicodeToArray.md)

## 源码分析

`stringToArray` 是将字符串转换成数组。

源码如下：

```javascript
function stringToArray(string) {
  return hasUnicode(string)
    ? unicodeToArray(string)
    : asciiToArray(string)
}
```

使用 `hasUnicode` 检测 `string` 中是否含有 `unicode` 字符，如果有，则使用 `unicodeToArray` 转换，否则使用 `asciiToArray` 转换。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 