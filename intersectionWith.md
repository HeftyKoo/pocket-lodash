# lodash源码分析之intersectionWith

本文为读 lodash 源码的第四十八篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

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

`intersectionWith` 的作用和 `intersection` 不同的是 `intersectionWith` 可以定义自己的比较函数。

它可以这样使用：

```javascript
intersectionWith([{a: 1, b: 2}], [{a: 1}], function (val1, val2) {
  return va1.a === val2.a
}) // [{a: 1, b: 2}]
```

源码实现如下：

```javascript
function intersectionWith(...arrays) {
  let comparator = last(arrays)
  const mapped = map(arrays, castArrayLikeObject)

  comparator = typeof comparator === 'function' ? comparator : undefined
  if (comparator) {
    mapped.pop()
  }
  return (mapped.length && mapped[0] === arrays[0])
    ? baseIntersection(mapped, undefined, comparator)
    : []
}
```

其找出 `comparator` 的过程和 `intersectionBy` 类似，即取出 `arrays` 的最后一项。

```javascript
comparator = typeof comparator === 'function' ? comparator : undefined
```

然后判断 `comparator` 是否为 `function`，如果为 `function` 则表示 `comparator` 有传，则要将 `mapped` 最后的一项取出，因为最后一项为 `comparator`。

最后将 `mapped` 和 `comparator` 传给 `baseIntersection` 函数找出交集。

## 相关链接

[《lodash源码分析之intersection》](intersection.md)

[《lodash源码分析之intersectionBy》](intersectionBy.md)

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 