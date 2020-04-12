# lodash源码分析之getSymbolsIn

本文为读 lodash 源码的第一百九十一篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import getSymbols from './getSymbols.js'
```

[《lodash源码分析之getSymbols》](./getSymbols.md)

## 源码分析

`getSymbolsIn` 和 `getSymbols` 的作用差不多，都是收集 `object` 上的 `Symbol` 类型的属性，但是 `getSymbolsIn` 和 `for...in` 一样，会遍历 `object` 原型链，收集原型链上所有的 `Symbol` 属性。

源码如下：

```javascript
function getSymbolsIn(object) {
  const result = []
  while (object) {
    result.push(...getSymbols(object))
    object = Object.getPrototypeOf(Object(object))
  }
  return result
}
```

使用 `Object.getPrototypeOf` 遍历 `object` 的原型链，每一层都调用 `getSymbols` 获取当前的 `Symbol` 属性，存入数组 `result` 中，最后将结果返回。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 