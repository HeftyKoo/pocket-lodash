# lodash源码分析之baseEachRight

本文为读 lodash 源码的第一百四十一篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseForOwnRight from './baseForOwnRight.js'
import isArrayLike from '../isArrayLike.js'
```

[《lodash源码分析之baseForOwnRight》](./baseForOwnRight.md)
[《lodash源码分析之isArrayLike》](../isArrayLike.md)

## 源码分析

`baseEachRight` 和 `baseEach` 的方法类似，不过是从后向前遍历。

源码如下：

```javascript
function baseEachRight(collection, iteratee) {
  if (collection == null) {
    return collection
  }
  if (!isArrayLike(collection)) {
    return baseForOwnRight(collection, iteratee)
  }
  const iterable = Object(collection)
  let length = collection.length

  while (length--) {
    if (iteratee(iterable[length], length, iterable) === false) {
      break
    }
  }
  return collection
}
```

和 `baseEach` 不太一样的地方有两处。

一是处理 `arrayLike` 时，调用的是 `baseForOwnRight` 而不是 `baseForOwn`。

二是 `while` 循环的中止条件是 `length--` ，索引从大到小，实现从后向前遍历。

`baseEach` 源码分析：[《lodash源码分析之baseEach》](./baseEach.md)

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 