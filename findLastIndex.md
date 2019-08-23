# lodash源码分析之findLastIndex

本文为读 lodash 源码的第三十六篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseFindIndex from './.internal/baseFindIndex.js'
```

[lodash源码分析之baseFindIndex中的运算符优先级](internal/baseFindIndex.md)

## 源码分析

```javascript
function findLastIndex(array, predicate, fromIndex) {
  const length = array == null ? 0 : array.length
  if (!length) {
    return -1
  }
  let index = length - 1
  if (fromIndex !== undefined) {
    index = fromIndex < 0
      ? Math.max(length + fromIndex, 0)
      : Math.min(fromIndex, length - 1)
  }
  return baseFindIndex(array, predicate, index, true)
}
```

### 作用与用法

从方法名可以看出， `findLastIndex` 会从后向前遍历，找到 `predicate` 第一次返回 `true` 时的 `index` 。

基本用法如下：

```javascript
 const users = [
   { 'user': 'barney',  'active': true },
   { 'user': 'fred',    'active': false },
   { 'user': 'pebbles', 'active': false }
 ]

 findLastIndex(users, ({ user }) => user == 'pebbles')
```

### 核心代码

```javascript
const length = array == null ? 0 : array.length
if (!length) {
  return -1
}
let index = length - 1
return baseFindIndex(array, predicate, index, true)
```

先不管 `fromIndex` 有传递的情况，即 `index` 为 `length - 1` ，即数组最后一项的索引。

从上面这段代码可以看出，`findLastIndex` 最终调用的是 `baseFindeIndex` ，并且将最后一个参数，即 `fromRight` 设置为 `true` ，表示从后向前遍历。

### fromIndex

默认情况下，是从最后一个元素开始向前遍历，也可以指定 `fromIndex` ，跳过部分元素。

`fromIndex` 可以是正数也可以是负数。

先看下负数的情况：

```javascript
Math.max(length + fromIndex, 0)
```

在负数的情况下，其实相当于 `length` 减少指定的数字，得到一个 `index`，这个 `index` 可能为负数，因为需要取`0` 和这个数字的最大值。

在正数的情况下：

```javascript
Math.min(fromIndex, length - 1)
```

这种情况下就比较简单了，取 `length - 1` 和 `fromIndex` 的最小值即可，避免超出数组的长度。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 