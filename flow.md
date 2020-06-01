# lodash源码分析之flow

本文为读 lodash 源码的第三百四十九篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`flow` 会接收一系列函数，并且会返回一个函数，当这个函数被调用时，会依照次序调用传入的函数，并且将上一个函数调用后得到的结果作为下一个函数的参数传入。最终返回最后一个函数的结果。

源码如下：

```javascript
function flow(...funcs) {
  const length = funcs.length
  let index = length
  while (index--) {
    if (typeof funcs[index] !== 'function') {
      throw new TypeError('Expected a function')
    }
  }
  return function(...args) {
    let index = 0
    let result = length ? funcs[index].apply(this, args) : args[0]
    while (++index < length) {
      result = funcs[index].call(this, result)
    }
    return result
  }
}
```

首先遍历一次 `funcs` ，以判断传入的所有参数是否都为函数，如果不是函数，则报 `TypeError` 错误。

可以看到，最终返回的结果是一个函数，当 `length` 为 `0` 时，会默认将 `args[0]` ，即传入这个函数的第一个参数作为结果返回。

当 `length` 大于 `0` 时，先调用 `funcs[0]` ，并且将所有参数传入这个函数，得到初始的结果 `result` 。

接着按顺序遍历余下的 `funcs` ，每次调用的时候，都将上一个结果 `result` 作为参数传入，得到新的结果，最后将最后一个函数的计算结果返回。

还可以看到， 所有 `funcs` 在调用的时候，绑定的 `this` 都是 `flow` 函数调用环境的 `this` 。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

