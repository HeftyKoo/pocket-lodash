# lodash源码分析之mapToArray

本文为读 lodash 源码的第二百一十五篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`mapToArray` 是将 `Map` 结构转换成 `[[key, value], ...]` 的数组。

源码如下：

```javacript
function mapToArray(map) {
  let index = -1
  const result = new Array(map.size)

  map.forEach((value, key) => {
    result[++index] = [key, value]
  })
  return result
}
```

先初始化一个和 `map` 的长度一样的数组。

然后调用 `map` 的 `forEach` 方法遍历 `map` ，将 `[key, value]` 存入 `result` 中。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 