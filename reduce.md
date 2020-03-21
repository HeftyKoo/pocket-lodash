# lodash源码分析之reduce

本文为读 lodash 源码的第一百三十四篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import arrayReduce from './.internal/arrayReduce.js'
import baseEach from './.internal/baseEach.js'
import baseReduce from './.internal/baseReduce.js'
```

[《lodash源码分析之arrayReduce》](internal/arrayReduce.md)
[《lodash源码分析之baseEach》](internal/baseEach.md)
[《lodash源码分析之baseReduce》](internal/baseReduce.md)

## 源码分析

`reduce` 的方法和数组的 `reduce` 方法签名一致，`lodash` 的 `reduce` 方法除了支持数组外，还支持对象。

源码如下：

```javascript
function reduce(collection, iteratee, accumulator) {
  const func = Array.isArray(collection) ? arrayReduce : baseReduce
  const initAccum = arguments.length < 3
  return func(collection, iteratee, accumulator, initAccum, baseEach)
}
```

先判断传入的 `collection` 是不是数组，如果是数组使用 `arrayReduce` ，否则使用 `baseReduce` 方法。

数组的 `reduce` 方法的第二个参数为初始值，这里对应的是第三个参数 `accumulator`。

所以使用 `arguments.length < 3` 来判断有没有传第三个参数，如果有传，则表示有传初始值，`initAccum` 为 `true`。

最后调用对应的 `func` ，将参数传入即可。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 