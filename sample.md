# lodash源码分析之sample

本文为读 lodash 源码的第一百六十二篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`sample` 的作用是从数组 `array` 中，随机取出其中的一个值。

源码如下：

```javascript
function sample(array) {
  const length = array == null ? 0 : array.length
  return length ? array[Math.floor(Math.random() * length)] : undefined
}
```

要从 `array` 中随机取值，只需要计算出随机的索引位置，然后再将该索引的值取出即可。

在 `javascript` 中，`Math.random` 方法可以得到 `0` 到小于 `1` 的浮点数，但是数组的索引是从 `0` 到小于 `length` 的整数。

因为 `Math.random` 是 `0` 小于 `1` 的浮点数，因此使用 `Math.random() * length` 即可得到从 `0` 到小于 `length` 的浮点数。

又索引必须是整数，则使用 `Math.floor` 即可对所得到的随机数取整，并且是向下取整，从而不会得到超过索引的值 `length` 。

对于空数组和传入 `null` 值或者 `undefined`，则直接返回 `undefined` 。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 