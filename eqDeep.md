# lodash源码分析之eqDeep

本文为读 lodash 源码的第二百二十篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseIsEqual from './.internal/baseIsEqual.js'
```

[《lodash源码分析之baseIsEqual》](internal/baseIsEqual.md)

## 源码分析

```javascript
function isEqual(value, other) {
  return baseIsEqual(value, other)
}
```

`eqDeep` 即以前的 `isEqual` ，其实就是调用 `baseIsEqual` 。


## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 