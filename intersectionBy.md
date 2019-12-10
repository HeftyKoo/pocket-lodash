# lodash源码分析之intersectionBy

本文为读 lodash 源码的第四十七篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import map from './map.js'
import baseIntersection from './.internal/baseIntersection.js'
import castArrayLikeObject from './.internal/castArrayLikeObject.js'
import last from './last.js'
```

[《lodash源码分析之map的实现》](map.md)

[《lodash源码分析之baseIntersection》](internal/baseIntersection.md)

[《lodash源码分析之castArrayLikeObject》](internal/castArrayLikeObject.md)

[《lodash源码分析之last》](last.md)

## 源码分析

`intersectionBy` 的作用和 `intersection` 相似， 也是找到传入数组的交集，但是它可以传多一个 `iteratee` 参数，内部同样调用 `baseIntersection` 来实现。

它可以这样使用：

```javascript
intersectionBy([2.1, 1.2], [2.3, 3.4], Math.floor) // [2.1]
```

源码实现如下：

```javascript
function intersectionBy(...arrays) {
  let iteratee = last(arrays)
  const mapped = map(arrays, castArrayLikeObject)

  if (iteratee === last(mapped)) {
    iteratee = undefined
  } else {
    mapped.pop()
  }
  return (mapped.length && mapped[0] === arrays[0])
    ? baseIntersection(mapped, iteratee)
    : []
}
```

它的后半部分和 `intersection` 类似，只不过在调用 `baseIntersection` 的时候传入了第二个参数 `iteratee` 。`intersection` 的分析见 [《lodash源码分析之intersection》](intersection.md)。

所不同的是前面提取 `iteratee` 参数的部分。

从使用方式可以看到，`intersectionBy` 的参数个数是不固定的，但是 `iteratee` 一定是最后一个参数。

所以第一句 `let iteratee = last(arrays)` 即取出最后一个参数，但是这个是否就是 `iteratee` 了呢？要知道，`iteratee` 是可选的，如果取出来就断定为 `iteratee` ，可能会造成错误。

所以就有了下面的判断：

```javascript
if (iteratee === last(mapped)) {
  iteratee = undefined
} else {
  mapped.pop()
}
```

如果 `iteratee === last(mapped)` ，如果经过数据全规化后的最后的值和原来的相同，表示最后一个值肯定是合法的数组，那肯定就不是 `iteratee` 了，因此需要将 `iteratee` 置为 `undefined`，否则 `iteratee` 有传，因此要调用 `mapped.pop()` 将最后一个值去掉。

最终将 `mapped` 和 `iteratee` 传给 `baseIntersection` 找出交集。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 