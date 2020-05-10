# lodash源码分析之baseInRange

本文为读 lodash 源码的第二百七十一篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`baseInRange` 用来检测 `number` 是否在 `start` 和 `end` 的范围内。

源码如下：

```javascript
function baseInRange(number, start, end) {
  return number >= Math.min(start, end) && number < Math.max(start, end)
}
```

即 `number` 需要同时满足不小于 `start` 和 `end` 的较小值，不大于 `start` 和 `end` 的较大者。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

