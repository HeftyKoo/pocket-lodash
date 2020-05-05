# lodash源码分析之toPlainObject

本文为读 lodash 源码的第二百四十九篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`toPlainObject` 的作用是将 `value` 转换成纯对象。

源码如下：

```javascript
function toPlainObject(value) {
  value = Object(value)
  const result = {}
  for (const key in value) {
    result[key] = value[key]
  }
  return result
}
```

可以看到，先使用对象字面量创建一个空对象 `result` 。

然后使用 `for...in` 将 `value` 上可枚举（包括原型链但除了 `Symbol` 类型）属性遍历，在 `result` 上设置值。

这有点像是将 `value` 复制给了 `result` ，从而得到一个纯对象，这个纯对象的构造函数是 `Object` 。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 