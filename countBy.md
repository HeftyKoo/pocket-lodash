# lodash源码分析之countBy

本文为读 lodash 源码的第一百三十五篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseAssignValue from './.internal/baseAssignValue.js'
import reduce from './reduce.js'
```

[《lodash源码分析之baseAssignValue》](internal/baseAssignValue.md)
[《lodash源码分析之reduce》](reduce.md)

## 源码分析

> 创建一个组成对象，key（键）是经过 `iteratee`（迭代函数） 执行处理`collection`中每个元素后返回的结果，每个key（键）对应的值是 `iteratee`（迭代函数）返回该key（键）的次数（注：迭代次数）。 iteratee 调用一个参数：*(value)*。

源码如下：

```javascript
const hasOwnProperty = Object.prototype.hasOwnProperty
function countBy(collection, iteratee) {
  return reduce(collection, (result, value, key) => {
    key = iteratee(value)
    if (hasOwnProperty.call(result, key)) {
      ++result[key]
    } else {
      baseAssignValue(result, key, 1)
    }
    return result
  }, {})
}
```

使用 `reduce` 方法来迭代 `collection`，每次迭代将 `value` 传给 `iteratee` 计算得到 `key` 。

然后判断 `result` 上是否已经存在 `key` ，如果存在则对应 `key` 的值加 `1` 。

否则表示 `key` 第一次遇到，使用 `baseAssignValue` 在 `result` 上添加 `key` 属性，其初始值为 `1` 。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 