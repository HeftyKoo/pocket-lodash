# lodash源码分析之differenceBy

> 走吧
>
> 落叶吹进深谷
>
> 歌声却没有归宿
>
>
>
> 走吧
>
> 冰上的月光
>
> 已从河床上溢出
>
>
>
> 走吧
>
> 眼睛望着同一片天空
>
> 心敲击着暮色的鼓
>
>
>
> 走吧
>
> 我们没有失去记忆
>
> 我们去寻找生命的湖
>
>
>
> 走吧
>
> 路啊路
>
> 飘满了红罂粟
>
>
>
> ——北岛《走吧》

本文为读 lodash 源码的第二十九篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

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

`differenceBy` 和 `difference` 的功能差不多，执行后，会返回一个新数组，新数组中的每一项都包含在第一个数组中，但是不包含在后面指定的数组中。

`difference` 的实现分析可以查看《[lodash源码分析之difference](./difference.md)》。

`differenceBy` 和 `difference` 的唯一不同是可以指定迭代器，如果有指定迭代器，则比较的是迭代器返回的值，并不是直接比较数组中的原始值。如果没有指定迭代器，则和 `difference` 完成相同。

下面来看下源码：

```javascript
function differenceBy(array, ...values) {
  let iteratee = last(values)
  if (isArrayLikeObject(iteratee)) {
    iteratee = undefined
  }
  return isArrayLikeObject(array)
    ? baseDifference(array, baseFlatten(values, 1, isArrayLikeObject, true), iteratee)
    : []
}
```

如果有指定迭代器，则 `differenceBy` 的最后一个参数必定不是数组。

因此使用 `last` 函数获取最后一个参数，然后使用 `isArrayLikeObject` 是否为类数组，如果是，则表明没有指定迭代器，将 `iteratee` 设置为 `undefined`。

最后跟 `difference` 一样，依然是调用 `baseDifference` 来进行比较，唯一不同的是，`baseDifference` 会传入第三个参数 `iteratee` 。

至于 `iteratee` 起到怎样的作用，那就需要看《[lodash源码分析之数组的差集](./internal/baseDifference.md)》了。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 