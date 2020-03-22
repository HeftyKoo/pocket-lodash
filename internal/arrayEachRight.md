# lodash源码分析之arrayEachRight

本文为读 lodash 源码的第一百三十八篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`arrayEachRight` 和 `arrayEach` 方法类似，不过是从后向前遍历，源码也大部分相同。

源码如下：

```javascript
function arrayEachRight(array, iteratee) {
  let length = array == null ? 0 : array.length

  while (length--) {
    if (iteratee(array[length], length, array) === false) {
      break
    }
  }
  return array
}
```

可以看到，只有 `while` 循环的中止条件不一样，这里使用 `length--` ，索引从大到小，即实现从后向前遍历。

`arrayEach` 源码分析：[《lodash源码分析之arrayEach》](./arrayEach.md)

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

