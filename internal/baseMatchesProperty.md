# lodash源码分析之baseMatchesProperty

本文为读 lodash 源码的第三百五十四篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseIsEqual from './baseIsEqual.js'
import get from '../get.js'
import hasIn from '../hasIn.js'
import isKey from './isKey.js'
import isStrictComparable from './isStrictComparable.js'
import matchesStrictComparable from './matchesStrictComparable.js'
import toKey from './toKey.js'
```

[《lodash源码分析之baseIsEqual》](./baseIsEqual.md)

[《lodash源码分析之get》](../get.md)

[《lodash源码分析之hasIn》](../hasIn.md)

[《lodash源码分析之isKey》](./isKey.md)

[《lodash源码分析之isStrictComparable》](./isStrictComparable.md)

[《lodash源码分析之matchesStrictComparable》](./matchesStrictComparable.md)

[《lodash源码分析之toKey》](./toKey.md)


## 源码分析

`baseMatchesProperty` 会接收一个属性路径 `path` 和一个值 `srcValue` ，返回一个函数，这个函数接收一个对象 `object` ，会检测 `object` 对应路径上的值和 `srcValue` 是否相等。

源码如下：

```javascript
const COMPARE_PARTIAL_FLAG = 1
const COMPARE_UNORDERED_FLAG = 2

function baseMatchesProperty(path, srcValue) {
  if (isKey(path) && isStrictComparable(srcValue)) {
    return matchesStrictComparable(toKey(path), srcValue)
  }
  return (object) => {
    const objValue = get(object, path)
    return (objValue === undefined && objValue === srcValue)
      ? hasIn(object, path)
      : baseIsEqual(srcValue, objValue, COMPARE_PARTIAL_FLAG | COMPARE_UNORDERED_FLAG)
  }
}
```

首先判断 `path` 是否为属性名，如果是，再判断 `srcValue` 是否可以使用 `===` 严格相等比较，如果可以，则使用 `matchesStrict	Comparable` 来生成函数。这样比接下来使用 `baseIsEqual` 来比较的性能要好。

如果不符合上面的条件，则返回一个接收 `object` 的函数。

在函数中，首先调用 `get` 方法，将 `object` 上对应路径 `path` 上的值 `objValue` 取出。

如果 `objValue` 为 `undefined` ，并且 `objValue` 和 `srcValue` 严格相等，则使用 `hasIn` 来判断 `object` 上是否存在 `path` 路径。因为 `object` 上 `path` 路径不存在，取出的 `objValue` 也是 `undefined` 。

其它情况，直接调用 `baseIsEqual` 来进行比较。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

