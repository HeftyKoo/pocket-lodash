# lodash源码分析之functions

本文为读 lodash 源码的第二百八十九篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`functions` 的作用是找出 `object` 中所有值的类型为函数的属性名。

源码如下：

```javascript
function functions(object) {
  if (object == null) {
    return []
  }
  return Object.keys(object).filter((key) => typeof object[key] === 'function')
}
```

如果没有传 `object` 或者传入 `null` ，返回一个空数组。

使用 `Object.keys` 获取 `object` 上所有的可枚举属性，然后用 `filter` 过滤出值类型为函数的属性。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 