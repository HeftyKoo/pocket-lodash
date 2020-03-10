# lodash源码分析之zipWith

本文为读 lodash 源码的第一百二十一篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import unzipWith from './unzipWith.js'
```

[《lodash源码分析之unzipWith》](unzipWith.md)

## 源码分析

`zipWith` 和 `unzipWith` 的作用差不多，但是 `zipWith` 接收的是不定参数，最后一个参数可以传入一个函数 `iteratee`，可以对每一个分组进行转换。

源码如下：

```javascript
function zipWith(...arrays) {
  const length = arrays.length
  let iteratee = length > 1 ? arrays[length - 1] : undefined
  iteratee = typeof iteratee === 'function' ? (arrays.pop(), iteratee) : undefined
  return unzipWith(arrays, iteratee)
}
```

如果 `arrays` 的长度大于 `1`，表示有可能传入了 `iteratee` 函数。

使用 `arrays[length - 1]` 将 `arrays` 最后一项取出，假设其为 `iteratee` 函数。这里其实也可以使用 `last` 函数来进行取值。

```javascript
iteratee = typeof iteratee === 'function' ? (arrays.pop(), iteratee) : undefined
```

这段其实可以改写成这样：

```javascript
if (typeof iteratee === 'function') {
  arrays.pop()
} else {
  iteratee = undefined
}
```

也即最后一个元素为 `function` 时，表示传入了 `iteratee` 函数，需要使用数组的 `pop` 方法，将数组中最后的值移除，否则将 `iteratee` 重新赋值为 `undefined`。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 