# lodash源码分析之baseEach

本文为读 lodash 源码的第一百三十二篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseForOwn from './baseForOwn.js'
import isArrayLike from '../isArrayLike.js'
```

[《lodash源码分析之baseForOwn》](./baseForOwn.md)
[《lodash源码分析之isArrayLike》](../isArrayLike.md)

## 源码分析

`baseEach` 的作用类似于数组的 `each` 方法，但是 `baseEach` 遍历的是对象，并不是数组。

源码如下：

```javascript
function baseEach(collection, iteratee) {
  if (collection == null) {
    return collection
  }
  if (!isArrayLike(collection)) {
    return baseForOwn(collection, iteratee)
  }
  const length = collection.length
  const iterable = Object(collection)
  let index = -1

  while (++index < length) {
    if (iteratee(iterable[index], index, iterable) === false) {
      break
    }
  }
  return collection
}
```

### 处理空值

如果没有传 `collection` 或者传入 `null` ，则直接返回 `collection`，不做任何处理。

### 处理非类数组

如果传入的 `collection` 为非类数组则使用 `baseForOwn` 来遍历。

### 处理类数组

如果为类数组，思路是获取 `collection` 的 `length` ，然后遍历索引即可。

注意一点，和数组的 `each` 不同的是，如果 `iteratee` 返回的结果为 `false`，则会中止遍历。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 