# lodash源码分析之lt

本文为读 lodash 源码的第二百三十九篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`lt` 用来比较传入的 `value` 值是否小于传入的 `other` 值 。

源码如下：

```javascript
function lt(value, other) {
  if (!(typeof value === 'string' && typeof other === 'string')) {
    value = +value
    other = +other
  }
  return value < other
}
```

和 `gt` 的逻辑类似，如果 `value` 和 `other` 不同时为 `string` 转换成 `number` 类型比较，如果同时为 `string`，则使用 `string` 的比较方式比较。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 