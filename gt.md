# lodash源码分析之gt

本文为读 lodash 源码的第二百零五篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`gt` 用来比较传入的 `value` 值是否比传入 `other` 值大。

源码如下：

```javascript
function gt(value, other) {
  if (!(typeof value === 'string' && typeof other === 'string')) {
    value = +value
    other = +other
  }
  return value > other
}
```

可以看到，在 `value` 和 `other` 不同时为 `string` 类型的时候，都转换成数字才进行比较。

如果 `value` 和 `other` 同时为 `string` ，则不进行转换，`>` 操作符会逐个字符转换成 `ascii` 比较，直至比较出全部字符相等，或者第一个不相等的字符为止，这个两个不相等字符的比较结果即为最终的比较结果。 

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 