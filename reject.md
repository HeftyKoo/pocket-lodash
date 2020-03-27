# lodash源码分析之reject

本文为读 lodash 源码的第一百六十一篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import filter from './filter.js'
import filterObject from './filterObject.js'
import negate from './negate.js'
```

[《lodash源码分析之filter》](./filter.md)
[《lodash源码分析之filterObject》](./filterObject.md)
[《lodash源码分析之negate》](./filterObject.md)

## 源码分析

`reject` 可以看作是 `filter` 的反操作。`filter` 会将 `predicate` 返回真值时的元素筛选出来，但是 `reject` 刚好想反，会将 `predicate` 返回假值时的元素筛选出来。

源码如下：

```javascript
function reject(collection, predicate) {
  const func = Array.isArray(collection) ? filter : filterObject
  return func(collection, negate(predicate))
}
```

如果传入的 `collection` 为数组，则使用 `filter` 函数，否则使用 `filterObject` 函数。

接下来，可以看到，`reject` 并不传直接将 `predicate` 传给 `filter` 或者 `filterObject` ，而是会使用 `negate` 包裹，也就是对 `predicate` 的结果进行取反。

所以 `reject` 最终得到的结果会和 `filter` 刚好相反。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 