# lodash源码分析之nthArg

本文为读 lodash 源码的第三百五十八篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import nth from './nth.js'
```

[《lodash源码分析之nth》](./nth.md)


## 源码分析

`nthArg` 其实是 `nth` 的偏应用，接收参数 `n` ，返回一个函数，调用这个函数时，会返回该函数的第 `n` 个参数。

源码如下：

```javascript
function nthArg(n) {
  return (...args) => nth(args, n)
}
```

可以看到，返回的函数其实就是调用 `nth` 来得到结果。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

