# lodash源码分析之arrayReduce

本文为读 lodash 源码的第一百二十二篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`arrayReduce` 的作用和数组的 `reduce` 的方法差不多。

源码如下：

```javascript
function arrayReduce(array, iteratee, accumulator, initAccum) {
  let index = -1
  const length = array == null ? 0 : array.length

  if (initAccum && length) {
    accumulator = array[++index]
  }
  while (++index < length) {
    accumulator = iteratee(accumulator, array[index], index, array)
  }
  return accumulator
}
```

`iteratee` 和 `reduce` 方法一样，接收上一次计算的值 `accumulator` ，当前值，当前 `index` 和传入的数组 `array` 。

`accumulator` 可以用来传入初始值。

`initAccum` 用来指定是否使用数组第一个值作为初始值。

```javascript
let index = -1
const length = array == null ? 0 : array.length

if (initAccum && length) {
  accumulator = array[++index]
}
```

如果 `initAccum` 为真值，并且数组不为空，则使用数组的第一个值作为初始值。

```javascript
while (++index < length) {
  accumulator = iteratee(accumulator, array[index], index, array)
}
```

接下来遍历数组，调用 `iteratee` 得到计算后的结果，保存在 `accumulator` 中。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 