# lodash源码分析之takeRightWhile

本文为读 lodash 源码的第九十四篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseWhile from './.internal/baseWhile.js'
```

[《lodash源码分析之baseWhile》](internal/baseWhile.md)

## 源码分析

`takeRightWhile` 的作用和 `takeRight` 差不多，但是不是直接传入数量 `n`，而是传入一个预测函数 `predicate`，只有在预测函数返回真值时才会从数组中将元素取出，和 `takeRight` 一样，`takeRightWhile` 也是从后向前取值的。

源码如下：

```javascript
function takeRightWhile(array, predicate) {
  return (array != null && array.length)
    ? baseWhile(array, predicate, false, true)
    : []
}
```

可以看到，`takeRightWhile` 其实就是直接调用 `baseWhile` 来实现的，注意传入的第四个参数 `fromRight` 为 `true` ，表示从后向前取值。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 