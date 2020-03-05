# lodash源码分析之filter

本文为读 lodash 源码的第一百零五篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

在 `es6` 大行其道的当下，相信大家对 `es5` 的 `filter` 的用法已经很熟悉的了。

它的作用是数组过滤，只将预测函数 `predicate` 返回真值时的元素过滤出来。

源码如下：

```javascript
function filter(array, predicate) {
  let index = -1
  let resIndex = 0
  const length = array == null ? 0 : array.length
  const result = []

  while (++index < length) {
    const value = array[index]
    if (predicate(value, index, array)) {
      result[resIndex++] = value
    }
  }
  return result
}
```

使用 `index` 作为迭代计数器，用 `resIndex` 作为结果集 `result` 的下标计数器。

然后遍历 `array` ，在迭代过程中，将元素 `value` 取出，然后将 `value` 和当前 `index` 及 `array` 传给 `predicate` 函数，如果返回真值，则将当前值存入结果集中。

最后将结果 `result` 返回。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 