# lodash源码分析之mapKey

本文为读 lodash 源码的第二百九十四篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`mapKey` 的作用是遍历 `object` 上可枚举的 `key` ，`iteratee` 会返回一个新的属性值来替换原来的属性。

源码如下：

```javascript
function mapKey(object, iteratee) {
  object = Object(object)
  const result = {}

  Object.keys(object).forEach((key) => {
    const value = object[key]
    result[iteratee(value, key, object)] = value
  })
  return result
}
```

使用 `Object.keys` 获取所有的可枚举属性，使用 `forEach` 遍历 `key` 。在遍历的过程中，调用 `iteratee` ，将当前值 `value` 和当前属性 `key` 及原 `object` 作为参数传入，`iteratee` 会返回一个新值替换原来的属性。

注意 `mapKey` 会返回一个新的对象，如果 `object` 是某个类的实例，原型链上的信息会丢失，并且不可枚举属性也会丢失。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 