# lodash源码分析之some

本文为读 lodash 源码的第二百一十三篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`some` 来数组的 `some` 方法的作用一样，可以传入断言函数，`some` 会遍历数组，在遍历的过程中会调用断言函数，只要断言函数返回真值，就会中止遍历，并且返回结果 `true` ，否则得到结果 `false` 。

源码如下：

```javascript
function some(array, predicate) {
  let index = -1
  const length = array == null ? 0 : array.length

  while (++index < length) {
    if (predicate(array[index], index, array)) {
      return true
    }
  }
  return false
}
```

可以看到，在遍历的过程中，会将当前值，当前索引和原数组传给断言函数 `predicate`。

如果 `array` 为空，得到的结果为 `false` ，因为根本没有机会调用断言函数。

## 参考资料

[MDN: Object.keys()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/keys)

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 