# lodash源码分析之xorWith

本文为读 lodash 源码的第一百一十三篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseXor from './.internal/baseXor.js'
import isArrayLikeObject from './isArrayLikeObject.js'
import last from './last.js'
```
[《lodash源码分析之baseXor》](internal/baseXor.md)
[《lodash源码分析之isArrayLikeObject》](isArrayLikeObject.md)
[《lodash源码分析之last》](last.md)

## 源码分析

`xorWith` 可以传入一个 `comparator` 比较函数。

源码如下：

```javascript
function xorWith(...arrays) {
  let comparator = last(arrays)
  comparator = typeof comparator === 'function' ? comparator : undefined
  return baseXor(arrays.filter(isArrayLikeObject), undefined, comparator)
}
```

同样使用 `...` 操作符将参数收集在 `arrays` 中。

`xorWith`也 要求 `comparator` 函数作为最后一个参数传入，因此使用 `last` 函数将其取出。

如果传入的 `comparator` 不是 `function` ，也要将 `comparator` 重置为 `undefined` 。

最后也是调用 `baseXor` ，此时传入第三个参数 `comparator` 。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 