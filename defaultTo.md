# lodash源码分析之defaultTo

本文为读 lodash 源码的第三百四十八篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`defaultTo` 接收两个参数 `value` 和 `defaultValue` ，如果 `value` 为 `null` 、`NaN` 或者 `undefined` 时，会返回 `defaultValue` ，否则返回 `value` 。

源码如下：

```javascript
function defaultTo(value, defaultValue) {
  return (value == null || value !== value) ? defaultValue : value
}
```

`value == null` 用来判断是否为 `null` 或者 `undefined` ， `value !== value` 用来判断是否为 `NaN` 。

如果为以上三种情况，返回 `defaultValue` ，否则返回 `value` 。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

