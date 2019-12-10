# lodash源码分析之intersection

本文为读 lodash 源码的第四十六篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import map from './map.js'
import baseIntersection from './.internal/baseIntersection.js'
import castArrayLikeObject from './.internal/castArrayLikeObject.js'
```

[《lodash源码分析之map的实现》](map.md)

[《lodash源码分析之baseIntersection》](internal/baseIntersection.md)

[《lodash源码分析之castArrayLikeObject》](internal/castArrayLikeObject.md)

## 源码分析

`intersection` 的作用是找到传入数组的交集，它的功能单一，内部调用 `baseIntersection` 来实现。

```javascript
function intersection(...arrays) {
  const mapped = map(arrays, castArrayLikeObject)
  return (mapped.length && mapped[0] === arrays[0])
    ? baseIntersection(mapped)
    : []
}
```

第一句 `const mapped = map(arrays, castArrayLikeObject)` 先将参数作一下合规化检测。

接下来的判断条件比较有趣，如果 `mapped.length` 为 `0` 则返回空数组，这比较好理解，传入的参数为空，肯定也没有交集。

接下来是 `mapped[0] === arrays[0]` ，为什么要判断 `mapped` 的第一项和原来的 `arrays` 的第一项是否相等呢？在合规性检测中，`castArrayLikeObject` 如果检测到数据不合规的时候会返回空数组，如果第一项就不合规，那返回的是空数组，那肯定会没有交集，因此可以直接返回空数组。

如果满足条件，则调用 `baseIntersection` 找到交集。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 