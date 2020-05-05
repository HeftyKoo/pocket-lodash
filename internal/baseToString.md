# lodash源码分析之baseToString

本文为读 lodash 源码的第二百五十三篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import isSymbol from '../isSymbol.js'
```

[《lodash源码分析之isSymbol》](../isSymbol.md)

## 源码分析

`baseToString` 的逻辑作为一个内部函数，作用和 `toString` 基本一样，之所以不直接使用 `toString` ，和 `baseToNumber` 一样，都基于同样的理由，出于性能的考虑。

源码如下：

```javascript
const INFINITY = 1 / 0
const symbolToString = Symbol.prototype.toString
function baseToString(value) {
  if (typeof value === 'string') {
    return value
  }
  if (Array.isArray(value)) {
    return `${value.map(baseToString)}`
  }
  if (isSymbol(value)) {
    return symbolToString ? symbolToString.call(value) : ''
  }
  const result = `${value}`
  return (result == '0' && (1 / value) == -INFINITY) ? '-0' : result
}
```

源码基本和 `toString` 一致，但是不会处理 `null` 和 `undefined` 。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

