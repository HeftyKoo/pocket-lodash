# lodash源码分析之times

本文为读 lodash 源码的第三百六十九篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`times` 接收两个参数 `n` 和 `iteratee` ，`times` 会调用 `iteratee`  `n` 次，`iteratee` 只接收一个参数，就是当前调用的索引 `index`，最终会得到一个由 `iteratee` 返回的结果组成的数组。

源码如下：

```javascript
const MAX_SAFE_INTEGER = 9007199254740991
const MAX_ARRAY_LENGTH = 4294967295
function times(n, iteratee) {
  if (n < 1 || n > MAX_SAFE_INTEGER) {
    return []
  }
  let index = -1
  const length = Math.min(n, MAX_ARRAY_LENGTH)
  const result = new Array(length)
  while (++index < length) {
    result[index] = iteratee(index)
  }
  index = MAX_ARRAY_LENGTH
  n -= MAX_ARRAY_LENGTH
  while (++index < n) {
    iteratee(index)
  }
  return result
}
```

### 处理n的边界值

```javascript
if (n < 1 || n > MAX_SAFE_INTEGER) {
  return []
}
```

当 `n` 小于 `1` 或者大于最大安全整数时，直接返回空数组，`iteratee` 也不会被调用。 

### 调用iteratee得到结果

```javascript
let index = -1
const length = Math.min(n, MAX_ARRAY_LENGTH)
const result = new Array(length)
while (++index < length) {
  result[index] = iteratee(index)
}
```

因为最终的结果是一个数组，因此结果的长度不能超过 `MAX_ARRAY_LENGTH` ，所以结果的长度为 `n` 和 `MAX_ARRAY_LENGTH` 的较小者。

接下来使用 `while` 循环，直至到达结果长度 `length` 时结束，在循环的过程中会调用 `iteratee` ，并将得到的结果存入 `result` 中。

### 疑惑

正常情况下，程序在上面就已经结束了，但是传入的 `n` 也可能会落在 `MAX_ARRAY_LENGTH` 到 `MAX_SAFE_INTEGER` 这个范围中，我的理解是，如果落在这个范围上，大于 `MAX_ARRAY_LENGTH` 的部分，`iteratee` 依旧会调用，但是调用的结果不再存入 `result` 中。

因此会得到以下的代码：

```javascript
while (++index < n) {
  iteratee(index)
}
```

但是 `times` 的源码是这样的：

```javascript
index = MAX_ARRAY_LENGTH
n -= MAX_ARRAY_LENGTH
while (++index < n) {
  iteratee(index)
}
```

为什么需要 `n -= MAX_ARRAY_LENGTH` 让我很疑惑，这样 `iteratee` 的调用肯定是不足 `n` 次的。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

