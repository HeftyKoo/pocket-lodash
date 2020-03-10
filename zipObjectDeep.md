# lodash源码分析之zipObjectDeep

本文为读 lodash 源码的第一百二十篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseSet from './.internal/baseSet.js'
import baseZipObject from './.internal/baseZipObject.js'
```

[《lodash源码分析之baseSet》](internal/baseSet.md)
[《lodash源码分析之baseZipObject》](internal/baseZipObject.md)

## 源码分析

`zipObjectDeep` 和 `zipObject` 的作用是差不多的，不过 `props` 里的每一项都支持属性路径。

```javascript
function zipObjectDeep(props, values) {
  return baseZipObject(props || [], values || [], baseSet)
}
```

可以看到 `baseZipObject` 的第三个参数和 `zipObject` 不同，`zipObject` 传的是 `assignValue` ，而 `zipObjectDeep` 传的是 `baseSet`，因为 `baseSet` 支持在属性路径上设置值。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 