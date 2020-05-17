# lodash源码分析之fromEntries

本文为读 lodash 源码的第二百八十五篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)


## 源码分析

`fromEntries` 的作用是将形如 `[[key1, value1], [key2, value2]]` 的结构转换成 `{key1: value1, key2: value2}` 的结构。

源码如下：

```javascript
function fromEntries(pairs) {
  const result = {}
  if (pairs == null) {
    return result
  }
  for (const pair of pairs) {
    result[pair[0]] = pair[1]
  }
  return result
}
```

先将结构设置为空对象，如果没有传入 `pairs` 或者传入的是 `null` ，则返回这个空对象。

如果有传 `pairs` ，则遍历 `pairs` ， `pair[0]` 即为属性， `pair[1]` 为值，要遍历的过程中，设置到结果 `result` 上。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 