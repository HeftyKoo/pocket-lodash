# lodash源码分析之baseAssignValue

本文为读 lodash 源码的第一百一十五篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`baseAssignValue` 的作用是将某个值 `value` 设置在 `object` 指定的 `key` 上。

在对象上设置某个 `key` 的值不是很简单吗？为什么需要写一个 `baseAssignValue` 的方法呢？

其实这个方法主要处理的是 `__proto__` 属性的赋值。

源码如下：

```javascript
function baseAssignValue(object, key, value) {
  if (key == '__proto__') {
    Object.defineProperty(object, key, {
      'configurable': true,
      'enumerable': true,
      'value': value,
      'writable': true
    })
  } else {
    object[key] = value
  }
}
```

`__proto__` 不是对象的一个标准属性，本来它应该和对象其他自定义属性一样，不应该有任何特别的，但是由于历史原因，基本所有的浏览器都实现了这个属性，用来保存对象的原型。

并且根据 `MDN` 的描述，这个属性还有如下特点：

> __proto__ 的设置器(setter)允许对象的 `[[Prototype]]被变更。前提是这个对象必须通过` [`Object.isExtensible()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/isExtensible) 判断为是可扩展的，如果不可扩展，则会抛出一个 [`TypeError`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/TypeError) 错误。要变更的值必须是一个object或[`null`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/null)，提供其它值将不起任何作用。

也就是说，像其它属性一样，直接给 `__proto__` 赋值，如果值不是对象，或者是不可扩展的对象，是可能会报错的。

而且 `__proto__` 也是不可枚举的。

因此，`lodash` 使用 `defineProperty` 来给 `__proto__` 来赋值，并且将 `__proto__` 的 `configurable` 、`enumerable` 、`writable` 都设置为 `true` ，这样 `__proto__` 和其他属性一样，都是可配置、可枚举、可写的属性了。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 