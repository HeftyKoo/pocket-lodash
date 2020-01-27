# lodash源码分析之pullAllWith

本文为读 lodash 源码的第六十四篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import basePullAll from './.internal/basePullAll.js'
```

[《lodash源码分析之basePullAll》](internal/basePullAll.md)

## 源码分析

```javascript
function pullAllWith(array, values, comparator) {
  return (array != null && array.length && values != null && values.length)
    ? basePullAll(array, values, undefined, comparator)
    : array
}
```

`pullAllBy` 和 `pullAll` 及 `pullAllBy` 的源码基本一样，在 `pullAllBy` 中，调用者可以传迭代器，`basePullAll` 会使用迭代器返回的新值比较，在 `pullAllWith` 中，调用者则可以直接传比较函数，`basePullAll` 在比较时，不再使用内置的比较函数，而是直接调用传入的比较函数。

`pullAll` 的源码分析见：[lodash源码分析之pullAll](pullAll.md)

`pullAllBy` 的源码分析见： [lodash源码分析之pullAllBy](pullAllBy.md)

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 