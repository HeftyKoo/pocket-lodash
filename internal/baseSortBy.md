# lodash源码分析之baseSortBy

本文为读 lodash 源码的第一百五十二篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`baseSortBy` 会对传入的数组排序，传入的数组的元素会有要求，每个元素都是对象，并且最终要返回的值要在 `value` 这个属性下。

源码如下：

```javascript
function baseSortBy(array, comparer) {
  let { length } = array

  array.sort(comparer)
  while (length--) {
    array[length] = array[length].value
  }
  return array
}
```

其实就是调用数组的 `sort` 方法，将 `comparer` 传入，将 `array` 排好序。

然后遍历数组，将每个元素从 `value` 上取出，相当于对数组 `array` 做了一次 `map` 。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 