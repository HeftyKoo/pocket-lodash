# lodash源码分析之trim

本文为读 lodash 源码的第三百三十八篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import castSlice from './.internal/castSlice.js'
import charsEndIndex from './.internal/charsEndIndex.js'
import charsStartIndex from './.internal/charsStartIndex.js'
import stringToArray from './.internal/stringToArray.js'
```

[《lodash源码分析之castSlice》](./internal/castSlice.md)

[《lodash源码分析之charsEndIndex》](./internal/charsEndIndex.md)

[《lodash源码分析之charsStartIndex》](./internal/charsStartIndex.md)

[《lodash源码分析之stringToArray》](./internal/stringToArray.md)

## 源码分析

`trim` 用来去除字符串 `string` 前后两端指定的字符串 `chars` ，如果没有指定 `chars` ，则 `chars` 默认为空格，即效果和字符串的原生方法 `trim` 一样。

源码如下：

```javascript
function trim(string, chars) {
  if (string && chars === undefined) {
    return string.trim()
  }
  if (!string || !chars) {
    return (string || '')
  }
  const strSymbols = stringToArray(string)
  const chrSymbols = stringToArray(chars)
  const start = charsStartIndex(strSymbols, chrSymbols)
  const end = charsEndIndex(strSymbols, chrSymbols) + 1

  return castSlice(strSymbols, start, end).join('')
}
```

### 处理chars没传的情况

```javascript
if (string && chars === undefined) {
  return string.trim()
}
```

可以看到，`chars` 没传的时候，其实就是直接调用字符串的原生方法 `trim` 来移除前后两端的空格。

### 处理字符串或chars为假值的情况

```javascript
if (!string || !chars) {
  return (string || '')
}
```

字符串或者 `chars` 为假值时，前后没字符可以移除，直接返回原字符串，如果原字符串为假值，则转换成空字符串。

### 移除前后指定的字符串

```javascript
const strSymbols = stringToArray(string)
const chrSymbols = stringToArray(chars)
const start = charsStartIndex(strSymbols, chrSymbols)
const end = charsEndIndex(strSymbols, chrSymbols) + 1

return castSlice(strSymbols, start, end).join('')
```

要移除前后在 `chars` 中的字符，只要找到前面第一个不在 `chars` 中的字符的位置，和后面第一个不在 `chars` 中字符的位置，将前后两个位置的字符截取出来即可。

这里先使用 `stringToArray` 将 `string` 和 `chars` 都转换成数组，再调用 `charsStartIndex` 找出前面第一个不在 `chars` 中字符的位置，调用 `charsEndIndex` 找出后面第一个不在 `chars` 中字符的位置。

再调用 `castSlice` 截取出这两个位置的字符，这里截取出来的是一个字符数组，再使用 `join` 来拼接成字符串。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

