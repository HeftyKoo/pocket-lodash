# lodash源码分析之property

本文为读 lodash 源码的第三百六十三篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseProperty from './.internal/baseProperty.js'
import basePropertyDeep from './.internal/basePropertyDeep.js'
import isKey from './.internal/isKey.js'
import toKey from './.internal/toKey.js'
```

[《lodash源码分析之baseProperty》](./internal/baseProperty.md)

[《lodash源码分析之basePropertyDeep》](./internal/basePropertyDeep.md)

[《lodash源码分析之isKey》](./internal/isKey.md)

[《lodash源码分析之toKey》](./internal/toKey.md)


## 源码分析

`property` 函数接收属性路径 `path` ，并创建一个函数返回，这个函数接收一个 `object` ，调用时，会从 `object` 上取出属性路径 `path` 的值。

```javascript
function property(path) {
  return isKey(path) ? baseProperty(toKey(path)) : basePropertyDeep(path)
}
```

如果传入的 `path` 本身就是属性名，则调用 `baseProperty` 来生成函数，否则，调用 `basePropertyDeep` 来生成函数。

`baseProperty` 生成的函数性能会更加好。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

