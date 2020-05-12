# lodash源码分析之defaults

本文为读 lodash 源码的第二百七十六篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import eq from './eq.js'
```

[lodash源码分析之eq](./eq.md)

## 源码分析

`defaults` 可以传入 `object` ，同时可以传入多个默认对象，如果 `object` 上没有默认对象上的属性或者属性值为 `undefined` ，则在 `object` 上设置默认对象上对应属性的值。

源码如下：

```javascript
const objectProto = Object.prototype
const hasOwnProperty = objectProto.hasOwnProperty

function defaults(object, ...sources) {
  object = Object(object)
  sources.forEach((source) => {
    if (source != null) {
      source = Object(source)
      for (const key in source) {
        const value = object[key]
        if (value === undefined ||
            (eq(value, objectProto[key]) && !hasOwnProperty.call(object, key))) {
          object[key] = source[key]
        }
      }
    }
  })
  return object
}
```

遍历默认对象，得到每个默认对象 `source` 。

如果默认对象不为 `null`，遍历默认对象及其原型键上的属性 `key` ，如果 `object` 上对应属性的值 `value` 为 `undefined` ，则将默认对象上对应属性的值设置到 `object` 上。

如果有值，则还要判断这个值是不是 `Object.prototype` 上的值，因为 `object[key]` 会顺着 `object` 的原型链上查找值，最终会查找到 `Object.prototype` 上，如果是 `Object.prototype` 上的值，则 `defaults` 也会认为 `object` 上该属性是没有设置值的。

这里直接使用 `eq` 来判断 `value` 是否和 `Object.prototype` 上对应的值是否相等。但是这样还不够，因为 `object` 上也可能存在一个和 `Object.prototype` 上相等的值，因此还需要调用 `Object.prototype.hasOwnProperty` 来检测，以排除这种情况 。 

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 