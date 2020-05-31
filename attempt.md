# lodash源码分析之attempt

本文为读 lodash 源码的第三百四十四篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import isError from './isError.js'
```

[《lodash源码分析之isError》](./isError.md)

## 源码分析

`attempt` 会调用传入的函数 `func` ，并将参数传入，最终会返回函数的调用结果，或者返回一个错误对象。

源码如下：

```javascript
function attempt(func, ...args) {
  try {
    return func(...args)
  } catch (e) {
    return isError(e) ? e : new Error(e)
  }
}
```

其实就是简单地调用 `func` ，并且将结果返回。

但是会捕获 `func` 的出错信息，如果出错信息本身就是一个错误对象，直接返回，如果不是，则将其包装成错误对象返回。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

