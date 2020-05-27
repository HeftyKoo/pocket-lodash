# lodash源码分析之padEnd

本文为读 lodash 源码的第三百二十八篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import createPadding from './.internal/createPadding.js'
import stringSize from './.internal/stringSize.js'
```

[lodash源码分析之createPadding](./internal/createPadding.md)

[lodash源码分析之stringSize](./internal/stringSize.md)


## 源码分析

`padEnd` 是在 `string` 的长度不足 `length` 的时候，在后面补全指定的字符串 `chars` ，得到一个长度为 `length` 的字符串，如果长度超过 `length` ，则不补全，得到的是原字符串。

源码如下：

```javascript
function padEnd(string, length, chars) {
  const strLength = length ? stringSize(string) : 0
  return (length && strLength < length)
    ? (string + createPadding(length - strLength, chars))
    : (string || '')
}
```

其实处理逻辑和 `pad` 很类似，只不过更简单。

先还是调用 `stringSize` 来得到 `string` 的长度，如果有传入大于 `0` 的长度 `length`，并且 `length` 比字符串的长度 `strLength` 要长，则使用 `length - strLength` 得到需要补全字符串的长度，然后调用 `createPadding` 来得到补全字符串，最后 `string` 拼接上需要补全的字符串即可。

否则返回的是原字符串，如果原字符串传入的是假值，则返回的是空字符串。 

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

