# lodash源码分析之setToArray

本文为读 lodash 源码的第九十六篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`setToArray` 的作用是将 `set` 结构的数据，转换成数组。

源码如下：

```javascript
function setToArray(set) {
  let index = -1
  const result = new Array(set.size)

  set.forEach((value) => {
    result[++index] = value
  })
  return result
}
```

其实就是使用 `set` 的 `forEach` 方法，将值遍历出来，然后一个个存入数组 `result` 中，最后将 `result` 返回。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 