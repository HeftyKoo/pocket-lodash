# lodash源码分析之uniq

本文为读 lodash 源码的第一百零二篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseUniq from './.internal/baseUniq.js'
```

[《lodash源码分析之baseUniq》](internal/baseUniq.md)

## 源码分析

`uniq` 其实就是简单地调用 `baseUniq` 来实现的。

源码如下：

```javascript
function uniq(array) {
  return (array != null && array.length)
    ? baseUniq(array)
    : []
}
```

在调用 `baseUniq` 之前，会对传入的参数 `array` 做了一次检测，如果没有传入，或者传入空数组，则返回空数组，否则调用 `baseUniq` 得出结果返回。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 