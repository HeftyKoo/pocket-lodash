# lodash源码分析之castArray

本文为读 lodash 源码的第一百八十篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`castArray` 会将传入的 `value` 转换成数组返回，如果传入的 `value` 已经是数组，则返回原值。

源码如下：

```javascript
function castArray(...args) {
  if (!args.length) {
    return []
  }
  const value = args[0]
  return Array.isArray(value) ? value : [value]
}
```

判断 `args` 的长度为 `0` ，直接返回空数组。

因为 `castArray` 只接收一个值，所以从 `args[0]` 取出 `value` 。

如果 `value` 已经是 `array` ，则直接返回，否则返回一个只包含 `value` 一个值的数组。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 