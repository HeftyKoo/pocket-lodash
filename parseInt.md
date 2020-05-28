# lodash源码分析之parseInt

本文为读 lodash 源码的第三百三十篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import root from './.internal/root.js'
```

[lodash源码分析之root](./internal/root.md)


## 源码分析

`parseInt` 的作用和原生的 `parseInt` 是类似的，但是会对字符串开头部分的空格移除才会进行转换。

```javascript
const reTrimStart = /^\s+/
const nativeParseInt = root.parseInt
function parseInt(string, radix) {
  if (radix == null) {
    radix = 0
  } else if (radix) {
    radix = +radix
  }
  return nativeParseInt(`${string}`.replace(reTrimStart, ''), radix || 0)
}
```

可以传一个字符串和进制，如果没有传入，默认为 `0` 。

最后会调用原生的 `parseInt` 函数，但是在将字符串传入之前，会使用正则将字符串前面的空格移除。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

