# lodash源码分析之upperFirst

本文为读 lodash 源码的第三百一十一篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import createCaseFirst from './.internal/createCaseFirst.js'
```

[lodash源码分析之createCaseFirst](./internal/createCaseFirst.md)

## 源码分析

`upperFirst` 的作用是将字符串的第一个字符转换成大写。

源码如下：

```javascript
const upperFirst = createCaseFirst('toUpperCase')
```

其实就是调用 `createCaseFirst` 函数得到一个新的函数，这个函数会对字符串的第一个字符调用 `toUpperCase` 方法，转换成大写字母。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 