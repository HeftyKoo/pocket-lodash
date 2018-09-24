# lodash源码分析之difference

> 一切充满了善，然而到处是不凑巧。既然是不凑巧，因之素朴的善终难免产生悲剧。
>
> ——沈从文

本文为读 lodash 源码的第二十七篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseDifference from './.internal/baseDifference.js'
import baseFlatten from './.internal/baseFlatten.js'
import isArrayLikeObject from './isArrayLikeObject.js'
```

[lodash源码分析之数组的差集](./internal/baseDifference.md)

[lodash源码分析之baseFlatten](./internal/baseFlatten.md)

[lodash源码分析之isArrayLikeObject](./isArrayLikeObject.md)

## 源码分析

好了，终于分析到 `difference` 了，这个函数的参数数量并不固定，每个参数都是数组，作用是找出第一个数组中，都不存在于后面所有数组中的项，并组成新的数组返回。

其实从本系列文章的第四篇开始，就跟 `difference` 函数有关，但是 `difference` 的代码只有寥寥几行，下面便是全部的源码:

```javascript
function difference(array, ...values) {
    return isArrayLikeObject(array)
        ? baseDifference(array, baseFlatten(values, 1, isArrayLikeObject, true))
    : []
}
```

其实不看前面的文章，单纯从这几个代码中，也大概能捋出 `difference` 的思路。

首先，如果 `array` 不是数组，那根本就没有比较的必要了，因此直接返回一个空数组。

因为使用了解构，因此 `values` 其实是一个二维数组，因此使用 `baseFlatten` 将二维数组展平成一维数组。

`difference` 的核心功能在 `baseDifference` 中，因为 `baseDifference` 比较的是两个数组，在将 `values` 展平后，就可以正常调用 `baseDifference` 了。

以上，便是 `baseDifference` 的全部分析了，要更详细的了解内部实现，从本系列的第四篇开始看起，就会一清二楚了。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 