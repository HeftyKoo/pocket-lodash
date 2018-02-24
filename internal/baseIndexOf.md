# lodash源码分析之baseIndexOf

> 他只是一个活着但不知道自己存在的人。
>
> ——卡尔维诺《不存在的骑士》

本文为读 lodash 源码的第十二篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseFindIndex from './baseFindIndex.js'
import baseIsNaN from './baseIsNaN.js'
import strictIndexOf from './strictIndexOf.js'
```

[《lodash源码分析之baseFindIndex中的运算符优先级》](baseFindIndex.md)

[《lodash源码分析之NaN不是NaN》](../eq.md)

[《lodash源码分析之strictIndexOf 》](strictIndexOf.md)

## 源码分析

```javascript
function baseIndexOf(array, value, fromIndex) {
  return value === value
    ? strictIndexOf(array, value, fromIndex)
    : baseFindIndex(array, baseIsNaN, fromIndex)
}
```

`baseIndexOf` 跟数组的 `indexOf` 方法有两处差别：

1. 不处理 `fromIndex` 为负数时的情况
2. 可以查找 `NaN`

这个函数是 lodash 的内部函数，将会给 `indexOf` 函数调用，`fromIndex` 为负数的情况将会有 `indexOf` 函数来处理，后续的文章会分析到。

数组的 `indexOf` 方法，遵循的是 [`Strict Equality Comparison`](http://ecma-international.org/ecma-262/7.0/#sec-strict-equality-comparison) 的比较方式，因此在查找 `NaN` 时，返回的都是 `-1` 。但是 `baseIndexOf` 会处理 `NaN` 。

在三元表达式中， `value === value` 的结果为 `false` 时，则该值是 `NaN`，调用 `baseFindIndex` 函数来查找索引，使用 `baseIsNaN` 函数来作为 `baseFindIndex` 的比较函数。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面