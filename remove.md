# lodash源码分析之remove

本文为读 lodash 源码的第七十九篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import basePullAt from './.internal/basePullAt.js'
```

[《lodash源码分析之basePullAt》](internal/basePullAt.md)

## 源码分析

`remove` 的作用跟 `pullAt` 的作用差不多，只不过 `pullAt` 只能传入 `indexes`，`remove` 传入的是一个断言函数 `predicate`，在遍历数组的过程中，如果 `predicate` 返回的值为真值，则将这个值从数组中删除。

和 `pullAt` 一样，`remove` 除了会更改原数组外，最终返回的是被删除的值 。

源码如下：

```javascript
function remove(array, predicate) {
  const result = []
  if (!(array != null && array.length)) {
    return result
  }
  let index = -1
  const indexes = []
  const { length } = array

  while (++index < length) {
    const value = array[index]
    if (predicate(value, index, array)) {
      result.push(value)
      indexes.push(index)
    }
  }
  basePullAt(array, indexes)
  return result
}
```

首先初始化 `result` 来保存被删除的值。

用 `indexes` 来保存将要从数组中删除值的索引。

判断 `array` 是否传入，或者是否为空，如果没有传入，或者传入的是空数组，则将空的 `result` 返回即可。

然后遍历数组，在遍历的过程中，将当前值 `value` 和当前索引 `index` 及原数组 `array` 传入 `predicate` 函数，如果 `predicate` 函数返回真值，表示这个值是需要删除的，那就将这个值存入结果数组 `result` 中，并且将要删除的索引存入 `indexes` 中。

注意，在遍历的过程中，是没有在原数组中进行值的删除的，只是保存要删除的索引。

在收集完成要删除的索引 `indexes` 后，调用 `basePullAt` 将对应的索引的值全部删除。

最后返回删除的值 `result` 。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 