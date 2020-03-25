# lodash源码分析之compareMultiple

本文为读 lodash 源码的第一百五十三篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import compareAscending from './compareAscending.js'
```

[《lodash源码分析之compareAscending》](./compareAscending.md)

## 源码分析

`compareMultiple` 大体相当于 `sort` 的 `compareFunction` 。

但是 `compareMultiple` 并不直接使用 `value` 比较，`object` 和 `other` 都包含 `criteria` 属性，`criteria` 为数组，每个值都是从 `value` 上从不同维度上取出。

因为 `compareMultiple` 其实就是实现了不同维度，按照维度的优先级来比较。

源码如下：

```javascript
function compareMultiple(object, other, orders) {
  let index = -1
  const objCriteria = object.criteria
  const othCriteria = other.criteria
  const length = objCriteria.length
  const ordersLength = orders.length

  while (++index < length) {
    const order = index < ordersLength ? orders[index] : null
    const cmpFn = (order && typeof order === 'function') ? order: compareAscending
    const result = cmpFn(objCriteria[index], othCriteria[index])
    if (result) {
      if (order && typeof order !== 'function') {
        return result * (order == 'desc' ? -1 : 1)
      }
      return result
    }
  }
  // Fixes an `Array#sort` bug in the JS engine embedded in Adobe applications
  // that causes it, under certain circumstances, to provide the same value for
  // `object` and `other`. See https://github.com/jashkenas/underscore/pull/1247
  // for more details.
  //
  // This also ensures a stable sort in V8 and other engines.
  // See https://bugs.chromium.org/p/v8/issues/detail?id=90 for more details.
  return object.index - other.index
}
```

`compareMultiple` 先遍历维度 `criteria` ，如果在同一维度下的比较结果为 `0` ，即在这个维度下无法确定具体的排序，再取下一个维度进行比较。

每个维度都可以自定义一个比较函数 `cmpFn` ，如果该维度没有传入 `cmpFn` ，则使用默认的 `compareAscending` 函数，即按升序排序。

调用 `cmpFn` 会得到一个 `result` ，这个 `result` 跟 `sort` 的 `compareFunction` 一样，会得到 `-1` 、 `0` 或者 `1` 这几个值。

如果为 `0` 表示在这个维度无法确定排序，如果为 `-1` 或者 `1` ，还需要判断当前的 `order` 是不是已经传入，当前的 `order` 如果传入的不是函数，还可以传入 `desc` 和 `esc` 两个字符串值，表示当前维度按照升序还是降序来排序。

如果 `order` 为 `desc` ，即按降序排列，则将 `result * -1` ，将计算出来的结果取反即可。

如果比较完所有的维度，还是没有得到排序结果，此时 `while` 循环结束，但是还没有结果返回，如果直接返回 `undefined` ，则在某些环境中结果会不正确，在 `v8` 中也不能确保稳定的顺序，因此使用 `object.index - other.index` 来返回一个值，这个值大部分情况下为 `0` 。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 