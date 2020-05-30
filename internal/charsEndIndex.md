# lodash源码分析之charsEndIndex

本文为读 lodash 源码的第三百三十六篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseIndexOf from './baseIndexOf.js'
```

[《lodash源码分析之baseIndexOf》](./baseIndexOf.md)

## 源码分析

`charsEndIndex` 是一个内部方法，会在 `trim` 和 `trimEnd` 中使用，作用是找出 `strSymbols` 字符串数组中，从后往前数，第一个不在 `charSymbols` 字符串数组中出现的字符的位置。

源码如下：

```javascript
function charsEndIndex(strSymbols, chrSymbols) {
  let index = strSymbols.length

  while (index-- && baseIndexOf(chrSymbols, strSymbols[index], 0) > -1) {}
  return index
}
```

先将 `index` 初始化为 `strSymbols` 的长度， `while` 循环中使用的是 `index--` ，即从 `strSymbols` 后面向前面遍历，在循环过程中，调用 `baseIndexOf` 检测当前位置的字符串是否在 `chrSymbols` 中，当出现第一个不存在的字符串时，即停止遍历。

此时的位置即从后往前数，第一个不出现在 `charSymbols` 的位置。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

