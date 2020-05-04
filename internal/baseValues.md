# lodash源码分析之baseValues

本文为读 lodash 源码的第二百四十五篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`baseValues` 的作用是将 `object` 指定的属性集 `props` 对应的值取出来，作为一个数组返回。

源码如下：

```javascript
function baseValues(object, props) {
  return props == null ? [] : props.map((key) => object[key])
}
```

如果 `props` 没有传入，或者传入 `null` ，则返回一个空数组。

否则使用数组的 `map` 方法遍历 `props`，并且从 `object` 上依次取出对应 `key` 的值。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 