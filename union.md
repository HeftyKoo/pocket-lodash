# lodash源码分析之union

本文为读 lodash 源码的第九十九篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseFlatten from './.internal/baseFlatten.js'
import baseUniq from './.internal/baseUniq.js'
import isArrayLikeObject from './isArrayLikeObject.js'
```

[《lodash源码分析之baseFlatten》](internal/baseFlatten.md)
[《lodash源码分析之baseUniq》](internal/baseUniq.md)
[《lodash源码分析之isArrayLikeObject》](isArrayLikeObject.md)

## 源码分析

`union` 的作用是将多个数组合并成一个数组，并且合成的数组已经去重，而且按照传入的顺序合并。

源码如下：

```javascript
function union(...arrays) {
  return baseUniq(baseFlatten(arrays, 1, isArrayLikeObject, true))
}
```

`union` 调用按照以下的方式调用：

```javascript
union([2, 3], [1, 2]) //  => [2, 3, 1]
```

因为用 `...` 解构，传入的参数会合并成一个二维数组 `[[2,3], [1,2]]` ，所以用 `baseFlatten` 先将二维数组 `arrays` 拍平，这就完成了合并的功能。

注意 `baseFlatten` 的第三个参数 `predicate` 为 `isArrayLikeObject` ，表示只有数组或者类数组才会通过检测，第四个参数 `isStrict` 为 `true` ，表示没有通过 `predicate` 检测的参数会被直接舍弃掉。

例如这样调用：

```javascript
union([2, 3], 1, [1, 2])
```

因为第二个参数不是数组，会被直接舍弃掉。

在将 `arrays` 拍平后，调用 `baseUniq` 去重，即可实现合并和去重的功能

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 