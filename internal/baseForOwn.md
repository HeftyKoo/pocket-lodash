# lodash源码分析之baseForOwn

本文为读 lodash 源码的第一百三十一篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseFor from './baseFor.js'
import keys from '../keys.js'
```

[《lodash源码分析之baseFor》](./baseFor.md)
[《lodash源码分析之keys》](../keys.md)

## 源码分析

`baseForOwn` 可以遍历 `object` 自身的 `key` 和 `value`，在遍历的过程中，会调用 `iteratee` ，并且将 `value`、`key` 和 `object` 作为参数传给 `iteratee`。

源码如下：

```javascript
function baseForOwn(object, iteratee) {
  return object && baseFor(object, iteratee, keys)
}
```

其实就是调用 `baseFor`，然后传入 `keys` 函数作为 `keysFunc` 获取 `object` 上所有的自身可枚举属性。 

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 