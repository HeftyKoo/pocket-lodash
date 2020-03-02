# lodash源码分析之unionWith

本文为读 lodash 源码的第一百零一篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

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

`unionWith` 的作用跟 `union` 差不多，但是 `unionWith` 支持传入比较函数 `comparator`，和 `unionBy` 一样，`unionWith` 也是支持不定参数，`comparator` 作为最后一个参数传入。

```javascript
function unionWith(...arrays) {
  let comparator = last(arrays)
  comparator = typeof comparator === 'function' ? comparator : undefined
  return baseUniq(baseFlatten(arrays, 1, isArrayLikeObject, true), undefined, comparator)
}
```

同样，也是用 `last` 函数将 `comparator` 取出，然后判断是否为 `function`，如果不是，则重置为 `undefined`。

然后调用在调用 `baseUniq` 时将 `comparator` 传入。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 