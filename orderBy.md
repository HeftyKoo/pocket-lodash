# lodash源码分析之orderBy

本文为读 lodash 源码的第一百五十五篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseOrderBy from './.internal/baseOrderBy.js'
```

[《lodash源码分析之baseOrderBy》](internal/baseOrderBy.md)

## 源码分析

`orderBy` 可以指定 `iteratees` ，在排序时，使用 `iteratees` 所返回的结果进行排序，也可以指定 `orders` ，可以是函数也可以是字符串，如果是字符串，则 `desc` 表示为降序，`asc` 为升序。

源码如下：

```javascript
function orderBy(collection, iteratees, orders) {
  if (collection == null) {
    return []
  }
  if (!Array.isArray(iteratees)) {
    iteratees = iteratees == null ? [] : [iteratees]
  }
  if (!Array.isArray(orders)) {
    orders = orders == null ? [] : [orders]
  }
  return baseOrderBy(collection, iteratees, orders)
}
```

如果 `collection` 为 `null` 或者没有传，则直接返回空结果集。

因为 `orderBy` 其实调用的就是 `baseOrderBy` ，`baseOrderBy` 要求 `iteratees` 和 `orders` 为数组，因此需要对这两个参数进行检测，如果为空，传入空数组，如果有值，但传入的不是数组，则传入只包含这个值的数组。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 