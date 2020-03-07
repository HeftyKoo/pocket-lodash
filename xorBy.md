# lodash源码分析之xorBy

本文为读 lodash 源码的第一百一十二篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

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

`xorBy` 和 `xor` 不同的是，可以传入一个 `iteratee` 迭代函数，在比较的时候，可以使用 `iteratee` 函数返回的值来比较。

源码如下：

```javascript
function xorBy(...arrays) {
  let iteratee = last(arrays)
  if (isArrayLikeObject(iteratee)) {
    iteratee = undefined
  }
  return baseXor(arrays.filter(isArrayLikeObject), iteratee)
}
```

同样使用 `...` 操作符将参数收集在 `arrays` 中。

`xorBy` 要求 `iteratee` 函数作为最后一个参数传入，因此使用 `last` 函数将其取出。

如果 `iteratee` 是数组或者类数组，则表明没有传入 `iteratee` 函数，因此需要将 `iteratee` 重置为 `undefined` 。

最后调用 `baseXor` ，但是比 `xor` 传多一个 `iteratee` 参数。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 