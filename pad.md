# lodash源码分析之pad

本文为读 lodash 源码的第三百二十七篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import createPadding from './.internal/createPadding.js'
import stringSize from './.internal/stringSize.js'
```

[lodash源码分析之createPadding](./internal/createPadding.md)

[lodash源码分析之stringSize](./internal/stringSize.md)


## 源码分析

`pad` 的作用是在 `string`  的长度不足 `length` 的时候，使用 `chars` 在 `pad` 的前后补全，最后得到长度为 `length` 的字符串。

源码如下：

```javascript
function pad(string, length, chars) {
  const strLength = length ? stringSize(string) : 0
  if (!length || strLength >= length) {
    return (string || '')
  }
  const mid = (length - strLength) / 2
  return (
    createPadding(Math.floor(mid), chars) +
    string +
    createPadding(Math.ceil(mid), chars)
  )
}
```

如果 `length` 为真值，则调用 `stringSize` 获取字符串 `string` 的长度，否则直接默认为 `0` ，因为接下来的逻辑可以看到， `length` 为假值时，其实是不需要对字符串进行补全处理的。

如果 `length` 不为真值，或者字符串的长度不小于 `length` ，则直接返回原字符串，原字符串为假值时，返回空字符串。

因为要前后补全，所以将 `length` 和字符串的长度 `strLength` 的差除以 `2` 得到前后需要补全的长度。

但是这个 `mid` 可能为小数，因此 `pad` 规则在这种情况下，前面的补全长度使用向下取整 `Math.floor`， 后面的补全长度使用向上取整 `Math.ceil`，即后面的补全长度比前面的长。

补全字符串都是调用 `createPadding` 得到，最终按顺序和 `string` 拼接起来即可。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

