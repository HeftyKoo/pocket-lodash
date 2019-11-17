# lodash源码分析之baseIntersection

本文为读 lodash 源码的第四十四篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import SetCache from './SetCache.js'
import arrayIncludes from './arrayIncludes.js'
import arrayIncludesWith from './arrayIncludesWith.js'
import map from '../map.js'
import cacheHas from './cacheHas.js'
```

[《lodash源码分析之缓存使用方式的进一步封装》](SetCache.md)

[《lodash源码分析之arrayIncludes》](arrayIncludes.md)

[《lodash源码分析之arrayIncludesWith》](arrayIncludesWith.md)

[《lodash源码分析之map的实现》](../map.md)

[《lodash源码分析之cacheHas》](cacheHas.md)

## 源码分析

### 作用

`baseIntersection` 的作用是找出多个数组之间的交集。

### 实现思路

多个数组的交集，最后返回的肯定是一个数组，因此先得有个容器，我们先来写出这个函数的第一个行代码

```javascript
function baseIntersection (arrays) {
  const result = []
}
```

接下来，我们确定一下交集 `result` 的最大长度，`result` 的长度肯定不会比传入的最短数组的长度长，因此只需要找出传入数组中最短数组的长度，即可确定交集的最大长度。

```javascript
function baseIntersection (arrays) {
  const othLength = arrays.length
  const result = []
  
  let maxLength = Infinity
  let othIndex = othLength
  
  while (othIndex--) {
    array = arrays[othIndex]
    maxLength = Math.min(array.length, maxLength)
  }
}
```



## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 