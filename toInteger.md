# lodash源码分析之toInteger

本文为读 lodash 源码的第五十四篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import toFinite from './toFinite.js'
```

[《lodash源码分析之toFinite》](toFinite.md)

## 源码分析

`toInteger` 的作用是将 `value` 转换成整数，如果是无限值，则转换为最大的正整数或负整数。

```javascript
function toInteger(value) {
  const result = toFinite(value)
  const remainder = result % 1

  return remainder ? result - remainder : result
}
```

首先，调用 `toFinite` ，将传入的 `value` 转换为数字和有限值。

如果一个数字带有小数，要转换为整数时，最简单的方法就是将小数部分减掉。如: `3.2 - 0.2` 即可得到整数 `3`。

因此，对 `result` 模 `1` 即可得到 `result` 的小数部分 `remainder` ，如果 `remainder` 为 `0` ，则原来的值就已经是整数，直接返回即可，否则，用原值减去小数值，即可得到整数，即 `result - remainder` 。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 