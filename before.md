# lodash源码分析之before

本文为读 lodash 源码的第一百七十二篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`before` 会创建一个调用 `func` 的函数，`func` 的 `this` 会绑定到这个函数的 `this`，`func` 的调用次数不会超过 `n` 次，如果超过，则会返回最后一次的结果。

源码如下：

```javascript
function before(n, func) {
  let result
  if (typeof func !== 'function') {
    throw new TypeError('Expected a function')
  }
  return function(...args) {
    if (--n > 0) {
      result = func.apply(this, args)
    }
    if (n <= 1) {
      func = undefined
    }
    return result
  }
}
```

首先对 `func` 进行检测，如果不是 `function` ，则报 `TypeError` 。

用 `result` 来保存每次调用的结果。

可以看到，返回的函数每次调用的时候，`n` 都会递减，在 `n` 大于 `0` 时，都会调用 `func`，并且更新结果 `result` ，并且将 `result` 返回。

在 `n <= 1` 时，会将 `func` 设置为 `undefined`， 释放内存。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 