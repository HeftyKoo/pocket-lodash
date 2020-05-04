# lodash源码分析之asciiToArray

本文为读 lodash 源码的第二百四十二篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`asciiToArray` 是将 `ascii` （不包含 `unicode` 编码）的字符串转换成数组的形式。

源码如下：

```javascript
function asciiToArray(string) {
  return string.split('')
}
```

其实就是调用字符串的 `split` 方法，将每个字符分隔出来。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 