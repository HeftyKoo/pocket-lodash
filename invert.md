# lodash源码分析之invert

本文为读 lodash 源码的第二百九十二篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`invert` 的作用是将对象 `object` 的值作为属性，将属性作为值来得到一个新的对象。如果值有重复，则会根据迭代的顺序，后面的属性会覆盖前面的。

源码如下：

```javascript
function invert(object) {
  const result = {}
  Object.keys(object).forEach((key) => {
    let value = object[key]
    if (value != null && typeof value.toString !== 'function') {
      value = toString.call(value)
    }
    result[value] = key
  })
  return result
}
```

使用 `Object.keys` 获取 `object` 上所有的可枚举属性，使用 `forEach` 遍历属性，在遍历的过程中获取对应的值 `value` 。

因为对象的属性需要为 `string` 类型，在直接设置对象的属性时，会隐式调用 `value` 的 `toString` 方法转换成字符串。

因此需要检测 `value` 上有没有 `toString` 这个方法，如果没有，则借用 `Object.prototype.toString` 来转换。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 