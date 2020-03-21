# lodash源码分析之baseReduce

本文为读 lodash 源码的第一百三十三篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`baseReduce` 的作用类似于数组的 `reduce` 方法，但是这个方法是用在对象上的。

源码如下：

```javascript
function baseReduce(collection, iteratee, accumulator, initAccum, eachFunc) {
  eachFunc(collection, (value, index, collection) => {
    accumulator = initAccum
      ? (initAccum = false, value)
      : iteratee(accumulator, value, index, collection)
  })
  return accumulator
}
```

因此是内部方法，所以对象的遍历方法 `eachFunc` 是可以自定义的，这个 `eachFunc` 和数组的 `each` 方法作用类似。

`accumulator` 可以保存每次遍历后计算出来的结果，在 `initAccum` 为假值的情况下，`accumulator` 参数可以作为计算的初始值。

因为在 `initAccum` 为假值时，会直接调用 `iteratee` ，然后将传入的 `accumulator` 作为第一个参数传给 `iteratee` ，后续的遍历， `iteratee` 的计算结果会覆盖掉 `accumulator` 的值。

在遍历结果后，会将最后一个计算结果 `accumulator` 返回，这样就实现了对象的 `reduce`。

也可以直接将 `initAccum` 设置为真值，这样，在第一次遍历的时候，会忽略 `accumulator` 的传值，会使用 `eachFunc` 第一交遍历出来的 `value` 作为初始计算结果。

注意在使得 `initAccum` 为真值时，第一次遍历过后，`initAccum` 会被重置为 `false`。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 