# lodash源码分析之flatten

本文为读 lodash 源码的第三十八篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseFlatten from './.internal/baseFlatten.js'
```

[lodash源码分析之baseFlatten](internal/baseFlatten.md)

## 源码分析

`flatten` 的作用是将数组展平，但是只展平一级。如：

```javascript
flatten([1, [2, [3, [4]], 5]]) // [1, 2, [3, [4]], 5]
```

源码如下：

```javascript
function flatten(array) {
  const length = array == null ? 0 : array.length
  return length ? baseFlatten(array, 1) : []
}
```

首先获取数组的长度，如果数组为 `null` 或者 `undefined`，则 `length` 默认为 `0`。

然后判断 `length` 是否不为 `0` 。如果为 `0` ，则返回空数组，否则调用 `baseFlatten` ，注意 `baseFlatten` 第二个参数 `depth` 为 `1` ，即只展开一层。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 