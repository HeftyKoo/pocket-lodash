# lodash源码分析之every

本文为读 lodash 源码的第一百四十三篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`every` 和数组的 `every` 方法类似。但是数组的 `every` 对于数组的稀疏部分不进行遍历，`lodash` 的 `every` 函数会对数组每一个元素都遍历。

如果对于所有元素，`predicate  `函数返回的都是真值，则结果为 `true`，只要 `predicate` 返回假值，则会中止遍历，结果为 `false` 。

源码如下：

```javascript
function every(array, predicate) {
  let index = -1
  const length = array == null ? 0 : array.length

  while (++index < length) {
    if (!predicate(array[index], index, array)) {
      return false
    }
  }
  return true
}
```

可以看到，如果 `length` 为 `0` 时，即 `array` 为 `undefined` 或者 `null` ，或者为空集合时，`every` 返回的结果为 `true` 。

否则就遍历 `array` ，将当前值 `array[index]` 、当前索引 `index` 和 `array` 传给 `predicate` 函数，如果返回假值，则中止遍历，结果为 `false` 。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 