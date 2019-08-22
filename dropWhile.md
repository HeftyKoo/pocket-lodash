# lodash源码分析之dropWhile

本文为读 lodash 源码的第三十五篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseWhile from './.internal/baseWhile.js'
```

[lodash源码分析之baseWhile](/internal/baseWhile.md)

## 源码分析

```javascript
function dropWhile(array, predicate) {
  return (array != null && array.length)
    ? baseWhile(array, predicate, true)
    : []
}
```

`dropWhile` 和 `dropRightWhile` 一样，内部也是调用 `baseWhile` 方法，两者唯一不同的是最后一个参数，也即 `fromRight` 参数，`dropWhile` 中没有传，也即为假值，表示从左往右遍历，因此两者唯一的区别在于遍历顺序。

`dropRightWhile` 的分析见: [lodash源码分析之dropRightWhile](dropRightWhile.md)

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 