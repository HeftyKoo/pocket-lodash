# lodash源码分析之basePropertyOf

本文为读 lodash 源码的第三百一十六篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`basePropertyOf` 会支持传入一个对象 `object` ，然后返回一个函数。

源码如下：

```javascript
function basePropertyOf(object) {
  return (key) => object == null ? undefined : object[key]
}
```

可以看到，返回的函数支持传入属性 `key` ，返回的是 `objec` 上对应属性 `key` 的值。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

