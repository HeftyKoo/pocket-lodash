# lodash源码分析之baseFindKey

本文为读 lodash 源码的第二百八十七篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)


## 源码分析

`baseFindKey` 和 `findKey` 的作用是一样的，都是找到传入的对象中第一个符合条件的属性。但是 `baseFindKey` 更加低层，它只负责迭代的过程，但不负责迭代方式，迭代方式由传入的 `eachFunc` 负责。

源码如下：

```javascript
function baseFindKey(collection, predicate, eachFunc) {
  let result
  eachFunc(collection, (value, key, collection) => {
    if (predicate(value, key, collection)) {
      result = key
      return false
    }
  })
  return result
}
```

`eachFunc` 负责迭代 `collection` ，得到 `value` 、 `key` 和原对象 `collection` 。

在迭代的过程中，调用断言函数 `predicate` ，返回真值时，将 `key` 赋值给 `result` ，得到第一个符合条件的属性值 `key` 。同时会 `return false` ，中止遍历。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 