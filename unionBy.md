# lodash源码分析之unionBy

本文为读 lodash 源码的第一百篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseFlatten from './.internal/baseFlatten.js'
import baseUniq from './.internal/baseUniq.js'
import isArrayLikeObject from './isArrayLikeObject.js'
import last from './last.js'
```

[《lodash源码分析之baseFlatten》](internal/baseFlatten.md)
[《lodash源码分析之baseUniq》](internal/baseUniq.md)
[《lodash源码分析之isArrayLikeObject》](isArrayLikeObject.md)
[《lodash源码分析之last》](last.md)

## 源码分析

`unionBy` 的作用跟 `union` 差不多，`unionBy` 支持 `iteratee` 参数。

```javascript
function unionBy(...arrays) {
  let iteratee = last(arrays)
  if (isArrayLikeObject(iteratee)) {
    iteratee = undefined
  }
  return baseUniq(baseFlatten(arrays, 1, isArrayLikeObject, true), iteratee)
}
```

因为 `unionBy` 支持不定参数，如果要传 `iteratee` ，则 `iteratee` 必须是最后一个参数。

因此用 `last` 函数将最后那个参数取出，然后用 `isArrayLikeObject` 判断一下是否为数组或者类数组，如果是，表明没有传递 `iteratee` ，将 `iteratee` 重置为 `undefined`，这时的作用和 `union` 完全一致。

`unionBy` 最后也调用 `baseUniq` ，将第二个参数 `iteratee` 传入。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 