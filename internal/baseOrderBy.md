# lodash源码分析之baseOrderBy

本文为读 lodash 源码的第一百五十四篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseEach from './baseEach.js'
import baseSortBy from './baseSortBy.js'
import baseGet from './baseGet.js'
import compareMultiple from './compareMultiple.js'
import isArrayLike from '../isArrayLike.js'
```

[《lodash源码分析之baseEach》](./baseEach.md)
[《lodash源码分析之baseSortBy》](./baseSortBy.md)
[《lodash源码分析之baseGet》](./baseGet.md)
[《lodash源码分析之compareMultiple》](./compareMultiple.md)
[《lodash源码分析之isArrayLike》](../isArrayLike.md)

## 源码分析

`baseOrderBy` 是用来实现 `orderBy` 的内部函数，会对参数进行一些合规化处理。

源码如下：

```javascript
const identity = (value) => value

function baseOrderBy(collection, iteratees, orders) {
  if (iteratees.length) {
    iteratees = iteratees.map((iteratee) => {
      if (Array.isArray(iteratee)) {
        return (value) => baseGet(value, iteratee.length === 1 ? iteratee[0] : iteratee)
      }

      return iteratee
    })
  } else {
    iteratees = [identity]
  }

  let criteriaIndex = -1
  let eachIndex = -1

  const result = isArrayLike(collection) ? new Array(collection.length) : []

  baseEach(collection, (value) => {
    const criteria = iteratees.map((iteratee) => iteratee(value))

    result[++eachIndex] = {
      criteria,
      index: ++criteriaIndex,
      value
    }
  })

  return baseSortBy(result, (object, other) => compareMultiple(object, other, orders))
}
```

### 处理 `iteratees`

`iteratees` 可以是函数集、字符串集，如果是字符串集，则应该为属性路径，`iteratee` 的作用是根据传入的 `value` 值，返回一个新值，来作为排序时的比较值。

有多少个 `iteratee`，就有多少个维度的比较。

`iteratees` 的合规化处理源码如下：

```javascript
if (iteratees.length) {
  iteratees = iteratees.map((iteratee) => {
    if (Array.isArray(iteratee)) {
      return (value) => baseGet(value, iteratee.length === 1 ? iteratee[0] : iteratee)
    }

    return iteratee
  })
} else {
  iteratees = [identity]
}
```

如果传入的是空数组，则 `iteratees` 默认为 `[identity]` ，即每个 `iteratee` 调用的时候，都是返回值本身。

如果 `iteratees` 传入的是 `[['a', 'b', 'c']]` 这样的路径数组，则表示要取 `value` 上的  `a.b.c` 路径上的值进行比较。

因为后面 `iteratee` 会使用函数调用的方式，因此这里使用一个函数来取值：

```javascript
if (Array.isArray(iteratee)) {
  return (value) => baseGet(value, iteratee.length === 1 ? iteratee[0] : iteratee)
}
```

`baseGet` 除了支持数组路径外，也支持如 `a.b.c` 这样的字符串路径，这时传入的 `iteratees` 的值类似于 `[['a.b.c']]` 。

因此判断 `iteratee.length` 是否为 `1` ，如果长度为 `1`，则将值取出，直接传入字符串给 `baseGet` 。

### 遍历 `collection` ，获取所有维度的比较值

如果 `collection` 为类数组，则先初始化一个长度与 `collection` 一致的结果集，用来保存排序后的结果。

接着，使用 `baseEach` 遍历 `collection` ，每个值 `value` 都传给所有的 `iteratee` 函数，计算出所有维度的比较值，结果存入 `criteria` 中，在排序的时候，会从 `criteria` 取出来比较。比较的过程在 `compareMultiple` 函数中已经分析。

最后就是调用 `baseSortBy` 使用 `compareMultiple` 作为比较函数来得到排序结果。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 