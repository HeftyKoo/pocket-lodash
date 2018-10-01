# lodash源码分析之differenceWith

> 黑夜给了我黑色的眼睛，我却用它寻找光明
>
> ——顾城《一代人》

本文为读 lodash 源码的第三十篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseDifference from './.internal/baseDifference.js'
import baseFlatten from './.internal/baseFlatten.js'
import isArrayLikeObject from './isArrayLikeObject.js'
import last from './last.js'
```

[lodash源码分析之数组的差集](./internal/baseDifference.md)

[lodash源码分析之baseFlatten](./internal/baseFlatten.md)

[lodash源码分析之isArrayLikeObject](./isArrayLikeObject.md)

[lodash源码分析之last](./last.md)

## 源码分析

`differenceWith` 和 `difference` 以及 `differenceBy` 的功能差不多，我们从上一篇文章《[lodash源码分析之differenceBy](./differenceBy.md)》中已经知道，`differenceBy` 可以指定迭代器，那 `differenceWith` 又有什么不同呢？

`differenceWith` 与 `difference` 不同的地方在于，`differenceWith` 可以自定义比较函数。如果不指定比较函数，则 `differenceWith` 的功能和 `difference` 完全相同。

`differenceWith` 和 `differenceBy` 的源码基本一致，如下：

```javascript
function differenceWith(array, ...values) {
  let comparator = last(values)
  if (isArrayLikeObject(comparator)) {
    comparator = undefined
  }
  return isArrayLikeObject(array)
    ? baseDifference(array, baseFlatten(values, 1, isArrayLikeObject, true), undefined, comparator)
    : []
}
```

可以看到，`comparator` 是直接传给 `baseDifference` 的，`comparator` 便是自定义的比较函数，具体的分析见《[lodash源码分析之数组的差集](./internal/baseDifference.md)》

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 