# lodash源码分析之findLast

本文为读 lodash 源码的第一百四十四篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import findLastIndex from './findLastIndex.js'
import isArrayLike from './isArrayLike.js'
```
[lodash源码分析之findLastIndex](./findLastIndex.md)
[lodash源码分析之isArrayLike](./isArrayLike.md)

## 源码分析

`findLast` 和 `find` 的作用类似，不过是从后向前遍历，同时可以指定 `fromIndex` 跳过一些元素。

源码如下：

```javascript
function findLast(collection, predicate, fromIndex) {
  let iteratee
  const iterable = Object(collection)
  if (!isArrayLike(collection)) {
    collection = Object.keys(collection)
    iteratee = predicate
    predicate = (key) => iteratee(iterable[key], key, iterable)
  }
  const index = findLastIndex(collection, predicate, fromIndex)
  return index > -1 ? iterable[iteratee ? collection[index] : index] : undefined
}
```

### 处理类数组

要获取第一个满足 `predicate` 的元素，只需要找到满足 `predicate` 的第一个索引即可。

之前已经有 `findLastIndex` 这个方法，可以查找满足 `predicate` 的第一个索引值。

单纯类数组的处理，代码如下：

```javascript
function findLast(collection, predicate, fromIndex) {
  const iterable = Object(collection)
  const index = findLastIndex(collection, predicate, fromIndex)
  return index > -1 ? iterable[index] : undefined
}
```

### 处理非类数组

`findLastIndex` 也可以用来处理对象，对象本来是没有顺序的，但是 `findLast` 会以对象的 `keys` 作为顺序。

处理对象部分的源码如下：

```javascript
let iteratee
if (!isArrayLike(collection)) {
  collection = Object.keys(collection)
  iteratee = predicate
  predicate = (key) => iteratee(iterable[key], key, iterable)
}
```

这里，`collection` 应该为对象的 `keys` ，用 `Object.keys` 来获取。

因为后面会使用 `findLastIndex` 来获取索引，此时 `collection` 已经 `keys` ，如果直接将 `predicate` 传给 `findLastIndex` ，第一个参数并不是 `value` ，而是 `key` ，和 `predicate` 所要求的参数不一致，因此需要改写传给`findLastIndex` 的 `predicate` 函数。

这里先将传入的 `predicate` 函数存入 `iteratee` 变量，然后将 `predicate` 函数改写如下：

```javascript
predicate = (key) => iteratee(iterable[key], key, iterable)
```

`findLastIndex` 在调用 `predicate` 函数时，会传入 `key` ，在改写的 `predicate` 里再调用传入的 `iteratee`（原来的 `predicate` ），将值 `iterable[key]` 、`key` 和原值 `iterable` 传入。

 除了这部分改写之外，拿到 `index` 后，对象还需要从 `collection` 里取出 `key` ，然后再根据 `key` 取出值。

如下：

```javascript
iterable[iteratee ? collection[index] : index]
```

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 