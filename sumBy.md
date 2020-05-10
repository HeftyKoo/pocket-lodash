# lodash源码分析之sumBy

本文为读 lodash 源码的第二百六十九篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseSum from './.internal/baseSum.js'
```

[lodash源码分析之baseSum](internal/baseSum.md)

## 源码分析

`sumBy` 用来计算一组数据的和，不过需要外部传入迭代器：

```javascript
function sumBy(array, iteratee) {
  return (array != null && array.length)
    ? baseSum(array, iteratee)
    : 0
}
```

其实就是调用 `baseSum` 不实现。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

