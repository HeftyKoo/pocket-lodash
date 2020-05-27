# lodash源码分析之padStart

本文为读 lodash 源码的第三百二十九篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import createPadding from './.internal/createPadding.js'
import stringSize from './.internal/stringSize.js'
```

[lodash源码分析之createPadding](./internal/createPadding.md)

[lodash源码分析之stringSize](./internal/stringSize.md)


## 源码分析

`padStart` 是在 `string` 的长度不足 `length` 的时候，在前面补全指定的字符串 `chars` ，得到一个长度为 `length` 的字符串，如果长度超过 `length` ，则不补全，得到的是原字符串。

源码如下：

```javascript
function padStart(string, length, chars) {
  const strLength = length ? stringSize(string) : 0
  return (length && strLength < length)
    ? (createPadding(length - strLength, chars) + string)
    : (string || '')
}
```

逻辑几乎和 `padEnd` 的一样，只不过将补全字符串拼接在 `string` 的前面。

具体可以参考 [《lodash源码分析之padEnd》](./padEnd.md) 

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

