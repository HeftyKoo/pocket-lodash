# lodash源码分析之copyArray

本文为读 lodash 源码的第五十九篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`copyArray` 接入两个参数 `source` 和 `array`，如果 `array` 不传，则默认为空数组。`copyArray` 的作用是将 `source` 中的每一项复制到 `array` 中。

看看源码的实现：

```javascript
function copyArray(source, array) {
  let index = -1
  const length = source.length

  array || (array = new Array(length))
  while (++index < length) {
    array[index] = source[index]
  }
  return array
}
```

首先，如果 `array` 没有传，则使用 `new Array` 创建一个跟 `source` 长度一样的数组。

然后遍历 `source` ，将 `source` 中的每一项都复制到 `array` 中。

最后将 `array` 返回。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 