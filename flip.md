# lodash源码分析之flip

本文为读 lodash 源码的第一百七十七篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`flip` 会创建一个函数，这个函数会调用传入的 `func` ，但是会将参数以传入的顺序相反传给 `func` 。

源码如下：

```javascript
function flip(func) {
  if (typeof func !== 'function') {
    throw new TypeError('Expected a function')
  }
  return function(...args) {
    return func.apply(this, args.reverse())
  }
}
```

先检测 `func` 是否为函数，如果不是，则报错。

在返回的函数里，使用 `args.reverse()` 将参数顺序置反，然后传给 `func` 。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 