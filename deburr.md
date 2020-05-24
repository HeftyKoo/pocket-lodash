# lodash源码分析之deburr

本文为读 lodash 源码的第三百一十八篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import deburrLetter from './.internal/deburrLetter.js'
```

[《lodash源码分析之deburrLetter》](./internal/deburrLetter.md)

## 源码分析

> 转换字符串`string`中[拉丁语-1补充字母](https://en.wikipedia.org/wiki/Latin-1_Supplement_(Unicode_block)#Character_table) 和 [拉丁语扩展字母-A](https://en.wikipedia.org/wiki/Latin_Extended-A) 为基本的拉丁字母，并且去除组合变音标记。

例如：

```javascript
deburr('déjà vu') // => 'deja vu'
```

源码如下：

```javascript
const reLatin = /[\xc0-\xd6\xd8-\xf6\xf8-\xff\u0100-\u017f]/g

const rsComboMarksRange = '\\u0300-\\u036f'
const reComboHalfMarksRange = '\\ufe20-\\ufe2f'
const rsComboSymbolsRange = '\\u20d0-\\u20ff'
const rsComboMarksExtendedRange = '\\u1ab0-\\u1aff'
const rsComboMarksSupplementRange = '\\u1dc0-\\u1dff'
const rsComboRange = rsComboMarksRange + reComboHalfMarksRange + rsComboSymbolsRange + rsComboMarksExtendedRange + rsComboMarksSupplementRange

const rsCombo = `[${rsComboRange}]`
const reComboMark = RegExp(rsCombo, 'g')

function deburr(string) {
  return string && string.replace(reLatin, deburrLetter).replace(reComboMark, '')
}
```

`reLatin` 用来匹配拉丁字母的 `unicode` 编码，但是数学运算符除外。

`reComboMark` 用来匹配组合变音标记。

`string.replace(reLatin, deburrLetter)` 将匹配到的 `unicode` 编码，使用 `deburrLetter` 取得拉丁基本字母替换。

`replace(reComboMark, '')` 是将组合变量标记去除。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

