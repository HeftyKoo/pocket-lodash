# lodash源码分析之keyBy

本文为读 lodash 源码的第一百五十一篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseAssignValue from './.internal/baseAssignValue.js'
import reduce from './reduce.js'
```

[《lodash源码分析之baseAssignValue》](internal/baseAssignValue.md)
[《lodash源码分析之reduce》](reduce.md)

## 源码分析

`keyBy` 会得到一个集合，会遍历 `collection`，在遍历的过程中，会调用 `iteratee`  得到 `key` ，对应的 `value` 为生成这个 `key` 的最后一个元素。

源码如下：

```javascript
function keyBy(collection, iteratee) {
  return reduce(collection, (result, value, key) => (
    baseAssignValue(result, iteratee(value), value), result
  ), {})
}
```

使用 `reduce` 遍历，在遍历的过程中，调用 `iteratee` ，并且将当前值 `value` 传入计算出 `key` ，然后用 `baseAssignValue` 将值设置为 `value` ，如果 `key` 的值已经存在，则会被最新值覆盖。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 