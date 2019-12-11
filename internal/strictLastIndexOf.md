# lodash源码分析之strictLastIndexOf

本文为读 lodash 源码的第四十九篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`strictLastIndexOf` 和 `strictIndexOf` 的作用类似，都是找出指定的某个值在数组中的 `index`，所不同的是，`strictIndexOf` 是从前向后遍历，`strictLastIndexOf` 则是从后向前遍历。

源码如下：

```javascript
function strictLastIndexOf(array, value, fromIndex) {
  let index = fromIndex + 1
  while (index--) {
    if (array[index] === value) {
      return index
    }
  }
  return index
}
```

和 `strictIndexOf` 类似，`strictLastIndexOf` 也可以指定 `fromIndex` ，从 `fromIndex` 开始向前遍历。

遍历的终止条件是 `while(index--)` ，可以看到，这里是用 `index--` 并不是用 `--index`，在终止遍历的时候，即 `index--` 为 `0` 时，遍历终止，此时 `index` 的值为 `-1` ，因此如果所传的 `value` 不在数组中时，最终返回的结果为 `-1` 。因为在一开始就需要将 `index` 减少 `1` ，因此要从 `fromIndex + 1` 的位置处开始遍历。

## 相关链接

[《lodash源码分析之strictIndexOf》](./strictIndexOf.md)

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 