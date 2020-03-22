# lodash源码分析之arrayEach

本文为读 lodash 源码的第一百三十六篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`arrayEach` 的作用和数组原生的 `forEach` 方法类似，但是 `arrayEach` 支持稀疏数组的遍历，并且 `iteratee` 返回 `false` 时，会中止遍历。

```javascript
function arrayEach(array, iteratee) {
  let index = -1
  const length = array.length

  while (++index < length) {
    if (iteratee(array[index], index, array) === false) {
      break
    }
  }
  return array
}
```

源码很简单，迭代数组，然后调用 `iteratee` ，将当前值、索引和数组传入，如果 `iteratee` 返回 `false` ，则中止遍历。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 