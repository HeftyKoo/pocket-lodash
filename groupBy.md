# lodash源码分析之groupBy

本文为读 lodash 源码的第一百四十八篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseAssignValue from './.internal/baseAssignValue.js'
import reduce from './reduce.js'
```
[lodash源码分析之baseAssignValue](internal/baseAssignValue.md)
[lodash源码分析之reduce](./reduce.md)

## 源码分析

`groupBy` 可以对传入的 `collection` 元素进行分组。`groupBy` 会对 `collection` 进行遍历，在遍历的过程中，会调用 `iteratee` 函数，`iteratee` 会返回一个 `key` ，如果 `key` 相同，则会分为同一组。

源码如下：

```javascript
const hasOwnProperty = Object.prototype.hasOwnProperty
function groupBy(collection, iteratee) {
  return reduce(collection, (result, value, key) => {
    key = iteratee(value)
    if (hasOwnProperty.call(result, key)) {
      result[key].push(value)
    } else {
      baseAssignValue(result, key, [value])
    }
    return result
  }, {})
}
```

可以看到，`groupBy` 使用 `reduce` 遍历 `collection`，调用 `iteratee` 时，会传入 `value` ，得到 `key` 。

如果结果对象 `result` 中，已经存在 `key` ，则直接将 `value` `push` 入 `result[key]` 中即可。

如果没有，则使用 `baseAssignValue` 在 `result` 中初始化 `key` ，值为数组 `[value]` 。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 