# lodash源码分析之delay

本文为读 lodash 源码的第一百七十六篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`delay` 和 `defer` 一样，也是用来异步执行一个函数，但是 `delay` 可以指定等待的时间。

源码如下：

```javascript
function delay(func, wait, ...args) {
  if (typeof func !== 'function') {
    throw new TypeError('Expected a function')
  }
  return setTimeout(func, +wait || 0, ...args)
}
```

也是调用 `setTimeout` 来延迟执行，但是可以设置延迟的时间 `wait`。

## 相关资料

[lodash源码分析之defer](defer.md)

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 