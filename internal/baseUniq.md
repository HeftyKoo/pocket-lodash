# lodash源码分析之baseUniq

本文为读 lodash 源码的第九十八篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import SetCache from './SetCache.js'
import arrayIncludes from './arrayIncludes.js'
import arrayIncludesWith from './arrayIncludesWith.js'
import cacheHas from './cacheHas.js'
import createSet from './createSet.js'
import setToArray from './setToArray.js'
```

[《lodash源码分析之缓存使用方式的进一步封装》](./SetCache.md)
[《lodash源码分析之arrayIncludes》](./arrayIncludes.md)
[《lodash源码分析之arrayIncludesWith》](./arrayIncludesWith.md)
[《lodash源码分析之cacheHas》](./cacheHas.md)
[《lodash源码分析之createSet》](./createSet.md)
[《lodash源码分析之setToArray》](./setToArray.md)

## 源码分析

`baseUniq` 的作用是数组去重，是实现 `uniq` 、`uniqBy` 和 `uniqWith` 的内部方法，因此除了需要支持基本的去重操作外，还要支持 `uniqBy` 的 `iteratee` 参数和 `uniqWith` 的 `comparator` 参数。

所有源码如下：

```javascript
const LARGE_ARRAY_SIZE = 200
function baseUniq(array, iteratee, comparator) {
  let index = -1
  let includes = arrayIncludes
  let isCommon = true

  const { length } = array
  const result = []
  let seen = result

  if (comparator) {
    isCommon = false
    includes = arrayIncludesWith
  }
  else if (length >= LARGE_ARRAY_SIZE) {
    const set = iteratee ? null : createSet(array)
    if (set) {
      return setToArray(set)
    }
    isCommon = false
    includes = cacheHas
    seen = new SetCache
  }
  else {
    seen = iteratee ? [] : result
  }
  outer:
  while (++index < length) {
    let value = array[index]
    const computed = iteratee ? iteratee(value) : value

    value = (comparator || value !== 0) ? value : 0
    if (isCommon && computed === computed) {
      let seenIndex = seen.length
      while (seenIndex--) {
        if (seen[seenIndex] === computed) {
          continue outer
        }
      }
      if (iteratee) {
        seen.push(computed)
      }
      result.push(value)
    }
    else if (!includes(seen, computed, comparator)) {
      if (seen !== result) {
        seen.push(computed)
      }
      result.push(value)
    }
  }
  return result
}
```

### 使用Set去重

在 `es6` 中，`Set` 的 `key` 是唯一的，因此在支持 `Set` 的环境下，可以通过先将传入的 `array` 转换成 `Set` 结构，再将 `Set` 转换回数组，即可达成去重的目的。

假如，`baseUniq` 只有一个参数，没有传迭代器 `iteratee` 和比较器 `comparator`，也即在最简单的情况下，是可以直接用这种方式来转换的。

相关的代码如下：

```javascript
if (comparator) {
    ...
  }
  else if (length >= LARGE_ARRAY_SIZE) {
    const set = iteratee ? null : createSet(array)
    if (set) {
      return setToArray(set)
    }
  }
```

为什么要在 `length >= LARGE_ARRAY_SIZE` 才采用这种方式呢？其实这个主要不是针对 `Set` 转换的，而是为了性能优化，在数组长度比较长的时候，会用到缓存，后面会分析到。

### 整体思路

如果不用 `Set`，要将数组去重要怎样做呢？

我们可以用一个数组 `result` 来放置去重后的结果，然后一个一个遍历原数组 `array`，在每次迭代时，检测当前值是否已经在结果集 `result` 中，如果没有在 `result` 中，则将当前值存入 `result` 中即可，否则直接跳过。

根据这个思路，很容易写出以下的代码：

```javascript
let index = -1

const { length } = array
const result = []

outer:
while(++index < length) {
  let value = array[index]
  value = value !== 0 ? value : 0
  let seenIndex = result.length
  while (seenIndex--) {
    if (result[seenIndex] === value) {
      continue outer
    }
  }
  result.push(value)
}

return result
```

因为正负`0` 的存在，所以在 `value` 为 `+0` 或者 `-0` 时，都转换成 `0` 。

但是这段代码还是有问题的，这里没有考虑到 `value` 为 `NaN` 的情况，我们知道，在 `js` 中 `NaN === NaN` 是 `false` ，所以这段代码返回的结果中，可能会有多个 `NaN` 的存在。

那这种情况要怎样比较呢？可以用 `lodash` 的内部方法 `arrayIncludes`，这个方法考虑到了 `NaN` 的情况。

代码改成如下：

```javascript
let index = -1
let includes = arrayIncludes

const { length } = array
const result = []

outer:
while(++index < length) {
  let value = array[index]
  value = value !== 0 ? value : 0
  
  if (value === value) {
    let seenIndex = result.length
    while (seenIndex--) {
      if (result[seenIndex] === value) {
        continue outer
      }
    }
    result.push(value)
  } else if (!includes(result, value)) {
    result.push(value)
  }
}

return result
```

### 传入迭代器 `iteratee` 的情况

在传入迭代器时，在进行比较的时候，会使用 `iteratee` 返回的值来进行比较。

这个要改起来也很简单，只需要将获取 `value` 的地方调用一下 `iteratee`  即可。

因为要比较的值和最后返回的值不一致，这时每次迭代的时候，就不能直接用 `result` 是否存在 `value` 来判断了，必须要用另外一个容器 `seen` 来保存 `iteratee` 返回的值。

代码改成如下：

```javascript
let index = -1
let includes = arrayIncludes

const { length } = array
const result = []
let seen = result

if (comparator) {
  ...
}
else if (length >= LARGE_ARRAY_SIZE) {
  ...
}
else {
  seen = iteratee ? [] : result
}

outer:
while (++index < length) {
  let value = array[index]
  const computed = iteratee ? iteratee(value) : value

  value = value !== 0 ? value : 0
  if (computed === computed) {
    let seenIndex = seen.length
    while (seenIndex--) {
      if (seen[seenIndex] === computed) {
        continue outer
      }
    }
    if (iteratee) {
      seen.push(computed)
    }
    result.push(value)
  }
  else if (!includes(seen, computed)) {
    if (seen !== result) {
      seen.push(computed)
    }
    result.push(value)
  }
}
return result
```

可以看到，在没有传 `iteratee` 的时候，`seen` 和 `result` 是同一个引用，即可上一节的逻辑完全一致。

### 传入比较器 `comparator` 的情况

在传入比较器的时候，就需要用到 `arrayIncludesWith` 的方法了，因为 `arrayIncludes` 并不接受 `comparator` 参数，而且必须要确保迭代的时候，每次都走入调用 `arrayIncludesWith` 函数的分支，这可以用一个状态位 `isCommon` 来表示。

```javascript
...
if (comparator) {
  isCommon = false
  includes = arrayIncludesWith
}
else if (length >= LARGE_ARRAY_SIZE) {
  ...
}
else {
  ...
}
outer:
while (++index < length) {
  ...
  value = (comparator || value !== 0) ? value : 0
  if (isCommon && computed === computed) {
    ...
  }
  else if (!includes(seen, computed, comparator)) {
    ...
  }
}
return result
```

`isCommon` 默认为 `true` ，在传入 `comparator` 的时候，`isCommon` 改为 `false`，并且将 `incldues` 设置为 `arrayIncludesWith` ，这样每次迭代的时候都会走调用 `includes` 的分支。

另外值得留意的是，如果有传 `comparator` ，`value` 是不会做正负零转换的，完全交由 `comparator` 自己处理。

### 大数组性能优化

在数组比较大的时候，如果使用 `arrayIncludes` 或者 `arrayIncludesWith` 或者双重循环去比较，性能都比较差，因为时间复杂度会达到 `O(n^2)` 。

这时可以用 `SetCache`  来优化性能。

```javascript
if (comparator) {
  ...
}
else if (length >= LARGE_ARRAY_SIZE) {
  isCommon = false
  includes = cacheHas
  seen = new SetCache
}
else {
  ...
}
```

因为需要使用 `SetCache` ，需要使用 `cacheHas` 判断数据是否已经存在，因此需要设置 `includes` ，也就要求后面迭代 `array` 的时候，需要每次都走 `includes` 调用的分支，也即要将 `isCommon` 设置为 `false` 。

## 参考资料

[MDN: Set](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set#Browser_compatibility)

[lodash.js createSet函数的疑问](https://segmentfault.com/q/1010000016627460)

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 