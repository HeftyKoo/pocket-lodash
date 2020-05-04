# lodash源码分析之iteratorToArray

本文为读 lodash 源码的第二百四十一篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`iteratorToArray` 的作用是将一个迭代器转换成数组 。

迭代器是一种访问数据的接口，有一个 `next` 方法来访问数据，返回的格式如下：

```javascript
it.next() // { value: "data", done: false }
it.next() // { value: undefined, done: true }
```

每次调用 `next` 时，会在 `value` 中得到数据，`done` 来表示迭代完成的状态，如果 `done` 为 `true` ，表示数据已经消费完毕。

源码如下：

```javascript
function iteratorToArray(iterator) {
  let data
  const result = []

  while (!(data = iterator.next()).done) {
    result.push(data.value)
  }
  return result
}
```

其实就是一直调用 `iterator.next` 方法，直至 `done` 为 `true` 为止，每次调用时，将 `data.value` 即数据 `push` 入结果数组中。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 