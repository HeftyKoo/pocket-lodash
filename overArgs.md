# lodash源码分析之overArgs

本文为读 lodash 源码的第一百七十九篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`overArgs` 接收一个函数 `func` 和一组函数 `transforms` ，返回一个函数，这个函数会调用 `func` ，在调用 `func` 之前，会将参数使用 `transforms` 对应位置的转换方法进行转换，会将转换后的结果传入 `func` 。

源码如下：

```javascript
function overArgs(func, transforms) {
  const funcsLength = transforms.length
  return function(...args) {
    let index = -1
    const length = Math.min(args.length, funcsLength)
    while (++index < length) {
      args[index] = transforms[index].call(this, args[index])
    }
    return func.apply(this, args)
  }
}
```

`funcLength` 为转换函数的个数。

使用 `Math.min` 来取得参数个数和转换函数个数的较小者，这样避免转换函数不足，或者参数太少产生无效转换的情况。

然后调用 `transforms` 对应位置的转换方法 ，传入对应位置的参数，得到结果。

最后调用 `func` ，传入转换后的参数。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 