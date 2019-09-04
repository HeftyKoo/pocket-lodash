# lodash源码分析之flattenDeep

本文为读 lodash 源码的第三十九篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseFlatten from './.internal/baseFlatten.js'
```

[lodash源码分析之baseFlatten](internal/baseFlatten.md)

## 源码分析

与 `flatten` 不同的是，`flattenDeep` 会将数组展平成一级，无论层次有多深。如：

```javascript
flattenDeep([1, [2, [3, [4]], 5]]) // [1, 2, 3, 4, 5]
```

源码如下：

```javascript
function flattenDeep(array) {
  const length = array == null ? 0 : array.length
  return length ? baseFlatten(array, INFINITY) : []
}
```

源码里唯一的区别是，`baseFlatten` 的第二个参数为 `INFINITY` ，即无限级展平。

`flatten` 源码如下：[lodash源码分析之flatten](flatten.md)

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 