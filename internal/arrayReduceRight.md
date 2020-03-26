# lodash源码分析之arrayReduceRight

本文为读 lodash 源码的第一百五十七篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`arrayReduceRight` 的作用和 `reduce` 作用差不多，只不过 `arrayReduceRight` 是从后向前遍历。

源码如下：

```javascript
function arrayReduceRight(array, iteratee, accumulator, initAccum) {
  let length = array == null ? 0 : array.length
  if (initAccum && length) {
    accumulator = array[--length]
  }
  while (length--) {
    accumulator = iteratee(accumulator, array[length], length, array)
  }
  return accumulator
}
```

可以看到源码基本和 `reduce` 一致，只不过在 `initAccum` 为 `true` 时，初始值默认为数组的最后的元素。

`while` 循环的条件为 `while(length--)` ，索引从大到小递减，实现了从后向前遍历。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 