# lodash源码分析之after

本文为读 lodash 源码的第一百七十一篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`after` 会返回一个函数，在调用这个函数 `n` 次后，会调用传入 `after` 的 `func` 。

源码如下：

```javascript
function after(n, func) {
  if (typeof func !== 'function') {
    throw new TypeError('Expected a function')
  }
  n = n || 0
  return function(...args) {
    if (--n < 1) {
      return func.apply(this, args)
    }
  }
}
```

首先对 `func` 进行检测，如果不是 `function` ，则报 `TypeError` 。

如果没有传 `n` 则 `n` 默认为 `0` ，也即返回的函数第一次调用时就会调用 `func` 。

可以看到，返回的函数每次调用的时候，`n` 都会递减，在 `n` 小于 `1` 时，会调用 `func` ，`func` 的 `this` 会绑定创建函数的 `this` 。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 