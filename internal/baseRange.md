# lodash源码分析之baseRange

本文为读 lodash 源码的第三百六十五篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)


## 源码分析

`baseRange` 用来根据 `start` 、 `end` 和 `step` 来创建一个数组，这个数组的所有数字都在 `start` 和 `end` 之间，并且相邻两个之间的差值是 `step` 。 一般情况下，如果 `start` 比 `end` 小，则从小大到排序，否则从大到小排序，如果 `fromRight` 为 `true` ，则排序方式和前面相反。

源码如下：

```javascript
function baseRange(start, end, step, fromRight) {
  let index = -1
  let length = Math.max(Math.ceil((end - start) / (step || 1)), 0)
  const result = new Array(length)

  while (length--) {
    result[fromRight ? length : ++index] = start
    start += step
  }
  return result
}
```

如果 `step` 没有传，则默认 `step` 为 `1` 。

使用 `(end - start) / step` 可以得到数组的个数，但是这个结果可能为小数，因此使用 `Math.ceil` 来取整，又因为计算出来的个数可能为负数，因此再用 `Math.max` 来取和 `0` 相比的较大值，避免出现个数为负数的情况。

使用 `while` 遍历，这时 `length` 是递减的， `index` 是递增的，如果 `fromRight` 为 `true` ，则使用 `length` 作为索引，否则使用 `index` ，因此 `fromRight` 为 `true` 时， 和正常的排序方式刚好相反。

每次遍历都将 `start` 都递加上 `step` ，如果 `step` 为正数，则递增，如果为负数，则递减。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

