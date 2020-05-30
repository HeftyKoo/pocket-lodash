# lodash源码分析之charsStartIndex

本文为读 lodash 源码的第三百三十七篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseIndexOf from './baseIndexOf.js'
```

[《lodash源码分析之baseIndexOf》](./baseIndexOf.md)

## 源码分析

`chartsStartIndex` 的作用的找出 `strSymbols`字符串 数组中第一个不在 `chrSymbols` 字符串数组中出现的字符的位置。

源码如下：

```javascript
function charsStartIndex(strSymbols, chrSymbols) {
  let index = -1
  const length = strSymbols.length

  while (++index < length && baseIndexOf(chrSymbols, strSymbols[index], 0) > -1) {}
  return index
}
```

和 `charEndIndex` 的逻辑基本一致，但是是从前向后遍历。

参考：[《lodash源码分析之charsEndIndex》](./charsEndIndex.md)

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

