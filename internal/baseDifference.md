# lodash源码分析之数组的差集

> 外部世界那些破旧与贫困的样子，可以使我内心世界得到平衡。
>
> ——卡尔维诺《烟云》

本文为读 lodash 源码的第十七篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 作用与用法

`baseDifference`  可以用来获取指定数组与另一个数组的差集。

这个函数是内部函数，是后面实现其它比较函数的核心函数。

`baseDifference` 的方法签名如下：

```javascript
baseDifference(array, values, iteratee, comparator)
```

第一和第二个参数是需要比较的两个数组；`iteratee` 可以返回一值映射值，比较时，可以使用映射的值来进行比较； `comparator` 是自定义比较函数，如果有传递，则调用自定义的比较函数来进行交集的比较。

## 依赖

```javascript
import SetCache from './SetCache.js'
import arrayIncludes from './arrayIncludes.js'
import arrayIncludesWith from './arrayIncludesWith.js'
import map from '../map.js'
import cacheHas from './cacheHas.js'
```

[lodash源码分析之缓存使用方式的进一步封装](SetCache.md)

[lodash源码分析之arrayIncludes](arrayIncludes.md)

[lodash源码分析之arrayIncludesWith ](arrayIncludesWith.md)

[lodash源码分析之map的实现 ](../map.md)

[lodash源码分析之cacheHas ](cacheHas.md)

## 源码分析

```javascript
const LARGE_ARRAY_SIZE = 200
function baseDifference(array, values, iteratee, comparator) {
  let includes = arrayIncludes
  let isCommon = true
  const result = []
  const valuesLength = values.length

  if (!array.length) {
    return result
  }
  if (iteratee) {
    values = map(values, (value) => iteratee(value))
  }
  if (comparator) {
    includes = arrayIncludesWith
    isCommon = false
  }
  else if (values.length >= LARGE_ARRAY_SIZE) {
    includes = cacheHas
    isCommon = false
    values = new SetCache(values)
  }
  outer:
  for (let value of array) {
    const computed = iteratee == null ? value : iteratee(value)

    value = (comparator || value !== 0) ? value : 0
    if (isCommon && computed === computed) {
      let valuesIndex = valuesLength
      while (valuesIndex--) {
        if (values[valuesIndex] === computed) {
          continue outer
        }
      }
      result.push(value)
    }
    else if (!includes(values, computed, comparator)) {
      result.push(value)
    }
  }
  return result
}
```

### iteratee的调用

```javascript
if (iteratee) {
  values = map(values, (value) => iteratee(value))
}
```

如果有传递 `iteratee` ，则先调用 `map` ，使用 `iteratee` 生成要比较数组的映射数组 `values`。

因为后面会有嵌套循环，避免重复调用 `iteratee` ，影响性能，所以一开始就需要生成 `values` 的映射数组。

### 性能优化

这里使用了 `isCommon` 来标志是否使用普通方式来处理。

```javascript
if (comparator) {
  includes = arrayIncludesWith
  isCommon = false
}
```

如果有传递比较函数，则将 `isCommon` 标记为 `false`，表示不用普通的方式来处理，后面可以看到，最后会使用 `includes` 方法来处理，也即 `arrayIncludesWith` 方法。

```javascript
else if (values.length >= LARGE_ARRAY_SIZE) {
  includes = cacheHas
  isCommon = false
  values = new SetCache(values)
}
```

如果不需要使用自定义的比较方式，并且数组较大时（这里限定了200），则使用 `SetCache` 类来缓存数组。

`SetChche` 其实使用的是 `Map/Set` 或者对象的方式来存储，避免大数组嵌套循环时造成的性能损耗。

 ### 循环比较

接下来就遍历第一个数组 `array`，将数组中的每一项和第二个数组的每一项比较。

```javascript
if (isCommon && computed === computed) {
  let valuesIndex = valuesLength
  while (valuesIndex--) {
    if (values[valuesIndex] === computed) {
      continue outer
    }
  }
  result.push(value)
}
else if (!includes(values, computed, comparator)) {
  result.push(value)
}
```

可以看到，如果 `isCommon` 没有标记为 `false`， 或者需要比较的值 `computed` 不为 `NaN` 时，都采用嵌套循环的方式来比较。循环完毕，没有在第二个数组中发现相同的项时，将该项存入数组 `result` 中。

如果 `isCommon` 为 `false` 或者需要比较的值为 `NaN` 时，则调用 `includes` 方法来比较。

由之前的分析得知：

* 如果指定 `comparator` ，则 `includes` 为 `arrayIncludesWith`
* 如果被比较的数组 `values` 的长度超过 `200` ，则 `includes` 为 `cacheHas`
* 否则，`includes` 为 `arrayIncludes`

### +0与-0的处理

在看代码的时候，有一段十分奇怪：

```javascript
value = (comparator || value !== 0) ? value : 0
```

这段代码的意思是，在没有提供 `comparator` 的情况下，如果 `value === 0` ，则将 `value` 赋值为 `0` 。

`value === 0` 时，可能为 `+0` 、`-0` 和 `0` ，lodash 为什么要将它们都转为 `0` 呢？

 后来看到 lodash 作者在 [issue](https://github.com/lodash/lodash/issues/3175) 中说，因为比较会用到 `Set` ，而 `Set` 是不能区分 `+0` 和 `-0` 的。

## 参考

[Lodash系列——difference函数源码解析](https://segmentfault.com/a/1190000012676868)

[value = (comparator || value !== 0) ? value : 0; does it work?](https://github.com/lodash/lodash/issues/3175)

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面