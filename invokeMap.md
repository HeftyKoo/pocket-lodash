# lodash源码分析之invokeMap

本文为读 lodash 源码的第一百五十篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseEach from './.internal/baseEach.js'
import invoke from './invoke.js'
import isArrayLike from './isArrayLike.js'
```

[《lodash源码分析之baseEach》](internal/baseEach.md)
[《lodash源码分析之invoke》](invoke.md)
[《lodash源码分析之isArrayLike》](isArrayLike.md)

## 源码分析

`invokeMap` 会调用 `path` 上的方法处理 `collection` 上的每个元素，返回一个数组，包含每次调用的结果。如果 `path` 是一个函数，则每次调用的时候，`this` 指向的是当前的元素。

源码如下：

```javascript
function invokeMap(collection, path, args) {
  let index = -1
  const isFunc = typeof path === 'function'
  const result = isArrayLike(collection) ? new Array(collection.length) : []

  baseEach(collection, (value) => {
    result[++index] = isFunc ? path.apply(value, args) : invoke(value, path, args)
  })
  return result
}
```

如果 `collection` 是一个类数组，则初始化一个长度为 `length` 的数组作为结果集 `result` ，否则初始化一个空数组。

然后调用 `baseEach` 来遍历 `collection` ，`path` 如果是函数，则通过 `apply` 来调用，`this` 绑定到当前元素 `value` ，如果 `path` 是属性路径，则直接调用 `invoke` 方法，得到的结果存入结果集 `result` 中。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 