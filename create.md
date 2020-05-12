# lodash源码分析之create

本文为读 lodash 源码的第二百七十五篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`create` 方法类似于 `Object.create` ，但是第二个参数和 `Object.create` 稍有不同， `Object.create` 要求传入的是属性描述，但是 `create` 方法传入的是对象。

源码如下：

```javascript
function create(prototype, properties) {
  prototype = prototype === null ? null : Object(prototype)
  const result = Object.create(prototype)
  return properties == null ? result : Object.assign(result, properties)
}
```

首先判断 `prototype` 是否为 `null` ，如果不为 `null` ，则使用 `Object` 转换成对象。

接着调用 `Object.create` 创建一个对象 `result` ，继承 `prototype` 。

如果有传入 `properties` ，则调用 `Object.assign` 将 `properties` 自身可枚举的属性复制到 `result` 上。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 