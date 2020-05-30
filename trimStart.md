# lodash源码分析之trimStart

本文为读 lodash 源码的第三百四十篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import castSlice from './.internal/castSlice.js'
import charsEndIndex from './.internal/charsEndIndex.js'
import stringToArray from './.internal/stringToArray.js'
```

[《lodash源码分析之castSlice》](./internal/castSlice.md)

[《lodash源码分析之charsStartIndex》](./internal/charsStartIndex.md)

[《lodash源码分析之stringToArray》](./internal/stringToArray.md)

## 源码分析

`trimStart` 是将字符串 `string` 前面指定的字符 `chars` 移除，如果没有指定 `chars` ，则默认为空格。

源码如下：

```javascript
const methodName =  ''.trimLeft ? 'trimLeft' : 'trimStart'
function trimStart(string, chars) {
  if (string && chars === undefined) {
    return string[methodName]()
  }
  if (!string || !chars) {
    return (string || '')
  }
  const strSymbols = stringToArray(string)
  const start = charsStartIndex(strSymbols, stringToArray(chars))
  return castSlice(strSymbols, start).join('')
}
```

处理过程基本和 `trim` 一致，但是不对后面的字符串处理，因此 `castSlice` 截取的时候，直接从第一个不在 `chars` 中字符的位置开始，直至最后一个字符。

在没有传 `chars` 的时候，行为和字符串原生的方法 `trimStart` 一致，这里做了个兼容，`trimLeft` 只是 `trimStart` 的别名。

`trim` 的分析参考：[《lodash源码分析之trim》](./trim.md)

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

