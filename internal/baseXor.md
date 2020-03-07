# lodash源码分析之baseXor

本文为读 lodash 源码的第一百一十篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseDifference from './baseDifference.js'
import baseFlatten from './baseFlatten.js'
import baseUniq from './baseUniq.js'
```
[《lodash源码分析之数组的差集》](./baseDifference.md)
[《lodash源码分析之baseFlatten》](./baseFlatten.md)
[《lodash源码分析之baseUniq》](./baseUniq.md)

## 源码分析

`baseXor` 的作用是，传入一个数组集 `arrays` ，找出这个数组集中，只在其中一个数组中存在的元素，组成新的数组作为结果返回。

`baseXor` 和其他很多 `base` 类的内部函数一样，也支持迭代器 `iteratee` 和 `comparator` 。

例如：

```javascript
baseXor([1,2, 4],[2, 3], [3, 4]) // => [1] 
```

源码如下：

```javascript
function baseXor(arrays, iteratee, comparator) {
  const length = arrays.length
  if (length < 2) {
    return length ? baseUniq(arrays[0]) : []
  }
  let index = -1
  const result = new Array(length)

  while (++index < length) {
    const array = arrays[index]
    let othIndex = -1

    while (++othIndex < length) {
      if (othIndex != index) {
        result[index] = baseDifference(result[index] || array, arrays[othIndex], iteratee, comparator)
      }
    }
  }
  return baseUniq(baseFlatten(result, 1), iteratee, comparator)
}
```

如果传入的 `arrays` 的 `length` 为 `0` ，则返回空数组即可。

如果传入一个数组，则使用 `baseUniq` 去重即可。

对应的代码如下：

```javascript
const length = arrays.length
if (length < 2) {
  return length ? baseUniq(arrays[0]) : []
}
```

即如果有多个数组，要怎样去找只出现在其中一个数组中的元素呢？

我们可以先找出第一个数组中，所有只出现在第一个数组中的元素，再找出第二个数组中，所有只出现在这个数组中的元素，依此类推，最后将所有这些元素合并在一起，再进行去重就可以啦。

那怎样去找所有只出现在第一个数组中的元素呢？

我们只需要找出第一个数组和第二个数组的差集，假设存在数组 `result[0]` 中，然后再找出 `result[0]` 和第三个数组的差集，得出的结果重新存入 `result[0]`，依此类推，最后得出的结果肯定是只出现在第一个数组中的所有元素。

具体的代码如下：

```javascript
let index = -1
const result = new Array(length)

while (++index < length) {
  const array = arrays[index]
  let othIndex = -1

  while (++othIndex < length) {
    if (othIndex != index) {
      result[index] = baseDifference(result[index] || array, arrays[othIndex], iteratee, comparator)
    }
  }
}
```

得出的 `result` 和 `arrays` 的长度一致。

因为 `result` 中每个元素都是数组，可以使用 `baseFlatten(result, 1)` 将 `result` 展平，然后再调用 `baseUniq` 去重，即可得出最终的结果。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 