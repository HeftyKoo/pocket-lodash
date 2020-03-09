# lodash源码分析之baseZipObject

本文为读 lodash 源码的第一百一十七篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`baseZipObject` 的作用是将两个数组 `props` 和 `values` 组成一个对象。`props` 中的值为对象的键，`values` 的中的值为对象的值，键和值一一对应，即 `props[0]` 和 `values[0]` 组成一对，`props[1]` 和 `values[1]` 组成一对，依此类推。

 `baseZipObject` 并不会直接在对象上设置值，而是使用 `assignFunc` 设置对象的值，这样作为内部函数，可以给更多的场景来使用。

源码如下：

```javascript
function baseZipObject(props, values, assignFunc) {
  let index = -1
  const length = props.length
  const valsLength = values.length
  const result = {}

  while (++index < length) {
    const value = index < valsLength ? values[index] : undefined
    assignFunc(result, props[index], value)
  }
  return result
}
```

可以看到，`baseZipObject` 以 `props` 的长度为基准遍历，将 `props` 中的值一个一个取出，也将 `values` 对应 `index` 的值也取出，如果 `values` 的长度比 `props` 的长度长，则后面的值会舍弃，如果比 `props` 的短，则不足的位置的值为 `undefined`。

然后将 `result` 和 `props` 当前索引的值 `props[index]` 及 `value` 传给 `assignFunc` 处理，由 `assignFunc` 设置 `result` 的键和值即可。

为什么不用 `values` 的长度为基准呢，因为 `props` 如果长度不足，则可能会造成有部分 `values` 中的值不知道如何处理，如果长度过长，则有部分 `props` 会被舍弃，都和预期不符合，因此长度要以 `props` 的长度为基准。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 