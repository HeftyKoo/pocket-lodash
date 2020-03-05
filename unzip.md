# lodash源码分析之unzip

本文为读 lodash 源码的第一百零七篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import filter from './filter.js'
import map from './map.js'
import baseProperty from './.internal/baseProperty.js'
import isArrayLikeObject from './isArrayLikeObject.js'
```

[《lodash源码分析之filter》](filter.md)
[《lodash源码分析之map的实现》](map.md)
[《lodash源码分析之baseProperty》](internal/baseProperty.md)
[《lodash源码分析之isArrayLikeObject》](isArrayLikeObject.md)

## 源码分析

`unzip` 的作用是接收一个二维数组 `array`，返回一个新的二维数组 `result`，`result` 中的第一个元素，是由 `array` 中所有的数组的第一个元素组成，`result` 的第二个元素，由 `array` 中所有数组的第二个元素组成，依此类推。

例如：

```javascript
unzip([['a', 1, true], ['b', 2, false]]) // => [['a', 'b'], [1, 2], [true, false]]
```

源码如下：

```javascript
function unzip(array) {
  if (!(array != null && array.length)) {
    return []
  }
  let length = 0
  array = filter(array, (group) => {
    if (isArrayLikeObject(group)) {
      length = Math.max(group.length, length)
      return true
    }
  })
  let index = -1
  const result = new Array(length)
  while (++index < length) {
    result[index] = map(array, baseProperty(index))
  }
  return result
}
```

### 空参数处理

```javascript
if (!(array != null && array.length)) {
  return []
}
```

如果没有传递 `array` 或者 `array` 为空数组，直接返回空数组即可。

### 过滤不合规元素及计算结果集长度

```javascript
let length = 0
array = filter(array, (group) => {
  if (isArrayLikeObject(group)) {
    length = Math.max(group.length, length)
    return true
  }
})
```

`unzip` 要求传入的是二维数组，因此 `array` 中的每一项元素都必须为数组或者类数组。

因此使用 `filter` 去过滤 `array` 中的元素，如果通不过 `isArrayLikeObject` 检测的，直接过滤掉。

同时，结果集 `result` 的长度必定和 `array` 所有元素中，长度最长的元素的长度一致。

因此在 `filter` 的过程中，不断比较元素的长度。

### 填充结果集

```javascript
let index = -1
const result = new Array(length)
while (++index < length) {
  result[index] = map(array, baseProperty(index))
}
```

接下来，创建长度为 `length` 的结果集 `result` ，这时，`result` 是空的，需要将它填充。

在迭代的过程中，通过 `map` 函数去遍历 `array`，使用 `baseProperty` 将 `array` 中所有元素对应 `index` 的值取出。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 