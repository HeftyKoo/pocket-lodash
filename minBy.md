# lodash源码分析之minBy

本文为读 lodash 源码的第二百六十四篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`minBy` 的作用是取出一组数据中的最小值，需要传入迭代器 `iteratee`。

源码如下：

```javascript
function minBy(array, iteratee) {
  let result
  if (array == null) {
    return result
  }
  let computed
  for (const value of array) {
    const current = iteratee(value)

    if (current != null && (computed === undefined
      ? (current === current && !isSymbol(current))
      : (current < computed)
    )) {
      computed = current
      result = value
    }
  }
  return result
}
```

源码和 `maxBy` 基本一致，只不过 `current < computed` 的判断逻辑和 `maxBy` 的正好相反。

源码分析参考：[lodash源码分析之minBy](maxBy.md)

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

