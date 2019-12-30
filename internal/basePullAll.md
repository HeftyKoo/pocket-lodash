# lodash源码分析之basePullAll

本文为读 lodash 源码的第六十篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseIndexOf from './baseIndexOf.js'
import baseIndexOfWith from './baseIndexOfWith.js'
import copyArray from './copyArray.js'
```

[《lodash源码分析之baseIndexOf》](./baseIndexOf.md)

[《lodash源码分析之baseIndexOfWith》](./baseIndexOfWith.md)

[《lodash源码分析之copyArray》](./copyArray.md)

## 源码分析

`basePullAll` 会被 `pullAll`、`pull`、`pullBy`、`pullWith` 等函数调用，是实现这些函数的基础。

它的作用是将数组 `array` 中存在于数组 `values` 中的元素移除。同时也支持迭代器 `iteratee` 来用返回新的比较值，也支持 `comparator` 自定义比较函数。

源码如下：

```javascript
function basePullAll(array, values, iteratee, comparator) {
  const indexOf = comparator ? baseIndexOfWith : baseIndexOf
  const length = values.length

  let index = -1
  let seen = array

  if (array === values) {
    values = copyArray(values)
  }
  if (iteratee) {
    seen = map(array, (value) => iteratee(value))
  }
  while (++index < length) {
    let fromIndex = 0
    const value = values[index]
    const computed = iteratee ? iteratee(value) : value

    while ((fromIndex = indexOf(seen, computed, fromIndex, comparator)) > -1) {
      if (seen !== array) {
        seen.splice(fromIndex, 1)
      }
      array.splice(fromIndex, 1)
    }
  }
  return array
}
```

### indexOf的选择

```javascript
const indexOf = comparator ? baseIndexOfWith : baseIndexOf
```

`indexOf` 的作用和数组原生的 `indexOf` 方法类似，返回某个值在数组中的索引，如果值不在数组中，则返回 `-1`，因此也可以用来检测某个值是否在数组中。

这里，如果有传自定义比较器，则使用 `baseIndexOfWith` 函数，因为 `baseIndexOfWith` 支持自定义的比较器来比较。

### copy values

```javascript
if (array === values) {
  values = copyArray(values)
}
```

为什么要在 `array` 和 `values` 一致时要将 `values` 复制一份呢，因为数组是一个引用类型，后续会对 `array` 做一些可变操作，如果不对 `values` 复制，则 `values` 也会跟着 `array` 一起变动，会造成循环时出错。

### 迭代器的使用

```javascript
if (iteratee) {
  seen = map(array, (value) => iteratee(value))
}
```

如果有传迭代器，表示调用方可能想使用新值来进行比较，这里使用 `iteratee` 返回的值组成新的数组 `seen` 。

### 执行移除操作

```javascript
while (++index < length) {
  let fromIndex = 0
  const value = values[index]
  const computed = iteratee ? iteratee(value) : value

  while ((fromIndex = indexOf(seen, computed, fromIndex, comparator)) > -1) {
    if (seen !== array) {
      seen.splice(fromIndex, 1)
    }
    array.splice(fromIndex, 1)
  }
}
```

我们先将代码简化一下，就比较好理解了。

```javascript
while (++index < length) {
  let fromIndex = 0
  const value = values[index]
  
  while ((fromIndex = indexOf(array, value, fromIndex)) > -1) {
    array.splice(fromIndex, 1)
  }
}
```

以上代码是逻辑不严谨的，但是可以很清楚地看到移除的逻辑，其实就是遍历 `values` ，然后用 `indexOf` 方法检测 `array` 中是否存在存在于 `values` 中的值，直至将存在于 `values` 中的值都删除完毕。

至于为什么会有 `computed`，是因为使用方可以传 `iteratee` 返回新的比较值。

```javascript
if (seen !== array) {
  seen.splice(fromIndex, 1)
}
```

也由于有 `iteratee` 的存在，真正比较的可能不是 `array` ，因此 `indexOf` 传入的是 `seen` 而不是 `array` ，如果`seen` 和 `array` 并不相同，在 `array` 执行移除操作时，`seen` 也要执行移除操作。 

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 