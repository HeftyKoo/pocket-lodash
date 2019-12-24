# lodash源码分析之baseIndexOfWith

本文为读 lodash 源码的第五十八篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`baseIndexOfWith` 和 `baseIndexOf` 的作用类似，都是找出所传 `value` 在数组中的索引，不过 `baseIndexOfWith` 需要传入比较函数 `comparator` 。

源码如下：

```javascript
function baseIndexOfWith(array, value, fromIndex, comparator) {
  let index = fromIndex - 1
  const { length } = array

  while (++index < length) {
    if (comparator(array[index], value)) {
      return index
    }
  }
  return -1
}
```

和 `baseIndexOfWith` 需要传入 `fromIndex` 指定从数组开始查找的位置，之所以要将 `fromIndex - 1` ，是因为在 `while` 循环时，需要先将 `index` 自增，因此开始时，需要减 `1` 作为补偿。

在循环的过程中，调用传入的比较函数 `comparator` 比较当前项和指定的 `value` ，如果 `comparator` 返回真值，则返回当前 `index`。

如果将 `array` 遍历完毕，则返回 `-1` 。

## 参考资料

[lodash源码分析之baseIndexOf](./baseIndexOf.md)

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 