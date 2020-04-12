# lodash源码分析之getSymbols

本文为读 lodash 源码的第一百八十九篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`getSymbols` 的作用是从一个对象 `object` 中取出自身属性中所有可枚举的 `Symbol` 类型属性。

源码如下：

```javascript
const propertyIsEnumerable = Object.prototype.propertyIsEnumerable
const nativeGetSymbols = Object.getOwnPropertySymbols
function getSymbols(object) {
  if (object == null) {
    return []
  }
  object = Object(object)
  return nativeGetSymbols(object).filter((symbol) => propertyIsEnumerable.call(object, symbol))
}
```

获取对象的属性可以用 `getOwnPropertyNames` ，但是 `getOwnPropertyNames` 不会返回 `Symbol` 类型的属性。

获取 `Symbol` 类型的属性可以使用 `Object.getOwnPropertySymbols` 。

在传入的 `object` 为 `null` 或者 `undefined` 时，肯定是没有 `Symbol` 类型的属性，返回空数组。

然后使用 `Object` 将传入的数据作一次转换，避免传入的 `object` 不是对象导致出错的情况。

然后调用 `nativeGetSymbols` 也即 `Object.getOwnPropertySymbols` 来获取所有的 `Symbol` 类型属性。

但是获取到的属性有些可能是不可枚举，因此用 `filter` 过滤，使用 `Object.prototype.propertyIsEnumerable` 来检测属性是否可以枚举，只将可枚举的属性过滤出来。  

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 