# lodash源码分析之flattenDepth

本文为读 lodash 源码的第四十篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseFlatten from './.internal/baseFlatten.js'
```

[lodash源码分析之baseFlatten](internal/baseFlatten.md)

## 源码分析

与 `flatten`  及 `flattenDeep` 不同的是，`flattenDepth` 可以指定展平的层级。如：

```javascript
flattenDepth([1, [2, [3, [4]], 5]], 2) // [1, 2, 3, [4], 5]
```

源码如下：

```javascript
function flattenDepth(array, depth) {
  const length = array == null ? 0 : array.length
  if (!length) {
    return []
  }
  depth = depth === undefined ? 1 : +depth
  return baseFlatten(array, depth)
}
```

可以看到，如果不传 `depth`，默认展平一级。

`flatten` 源码分析： [lodash源码分析之flatten](flatten.md)

`flattenDeep` 源码分析： [lodash源码分析之flattenDeep](flattenDeep.md)

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 