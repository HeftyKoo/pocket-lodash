# lodash源码分析之sum

本文为读 lodash 源码的第二百六十八篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseSum from './.internal/baseSum.js'
```

[lodash源码分析之baseSum](internal/baseSum.md)

## 源码分析

`sum` 用来计算一组数据的和，源码如下：

```javascript
function sum(array) {
  return (array != null && array.length)
    ? baseSum(array, (value) => value)
    : 0
}
```

如果 `array` 没有传入，或者传入的是空数组，则直接返回 `0` 。

否则调用 `baseSum` 来计算结果，传入的迭代器 `iteratee` 总是返回原数据。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

