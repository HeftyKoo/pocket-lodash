# lodash源码分析之negate

本文为读 lodash 源码的第一百六十篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`negate` 函数会对 `predicate` 的结果进行取反，`predicate` 在调用的时候，`this` 会绑定到所创建的函数，并传入对应的参数。

```javascript
function negate(predicate) {
  if (typeof predicate !== 'function') {
    throw new TypeError('Expected a function')
  }
  return function(...args) {
    return !predicate.apply(this, args)
  }
}
```

首先判断 `predicate` 是否为函数，如果不同函数，则会报 `TypeError` 错误。

`negate` 返回的结果是一个函数，这个函数会调用 `predicate` ，并且将对应的参数传入，得到结果后取反。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 