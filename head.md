# lodash源码分析之head

本文为读 lodash 源码的第三十七篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`head` 和 `last` 刚好相反，获取的是数组第一项。

其实如果知道输入项一定是数组，直接使用 `array[0]` 来获取也很简单，`head` 方法主要做了一些非空的判断操作。

```javascript
function head(array) {
  return (array != null && array.length)
    ? array[0]
    : undefined
}
```

`last` 的源码分析如下：

[lodash源码分析之last](last.md)

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 