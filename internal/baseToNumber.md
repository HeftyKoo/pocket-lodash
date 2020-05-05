# lodash源码分析之baseToNumber

本文为读 lodash 源码的第二百五十二篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import isSymbol from '../isSymbol.js'
```

[《lodash源码分析之isSymbol》](../isSymbol.md)

## 源码分析

`baseToNumber` 的作用和 `toNumber` 差不多，都是将 `value` 转换成 `number` 类型。

但是 `baseToNumber` 作为一个内部方法，它的入参或用途是明确的，因此不会处理太多的特例。

`baseToNumber` 不会保证二进制、八进制或者十六进制形式的字符串会被正确地转换成对应的 `number` 类型。

这样处理的原因也是为了性能，不需要做太多无效的判断。

源码如下：

```javascript
function baseToNumber(value) {
  if (typeof value === 'number') {
    return value
  }
  if (isSymbol(value)) {
    return NAN
  }
  return +value
}
```

如果本身就是数字类型，直接返回本身 `value` 。

如果是 `Symbol` 类型，则返回 `NaN`。

其实类型，使用 `+value` 进行隐式转换。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

