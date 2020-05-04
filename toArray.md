# lodash源码分析之toArray

本文为读 lodash 源码的第二百四十七篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import copyArray from './.internal/copyArray.js'
import getTag from './.internal/getTag.js'
import isArrayLike from './isArrayLike.js'
import isString from './isString.js'
import iteratorToArray from './.internal/iteratorToArray.js'
import mapToArray from './.internal/mapToArray.js'
import setToArray from './.internal/setToArray.js'
import stringToArray from './.internal/stringToArray.js'
import values from './values.js'
```

[《lodash源码分析之copyArray》](internal/copyArray.md)

[《lodash源码分析之getTag》](internal/getTag.md)

[《lodash源码分析之isArrayLike》](isArrayLike.md)

[《lodash源码分析之isString》](isString.md)

[《lodash源码分析之iteratorToArray》](internal/iteratorToArray.md)

[《lodash源码分析之mapToArray》](internal/mapToArray.md)

[《lodash源码分析之setToArray》](internal/setToArray.md)

[《lodash源码分析之stringToArray》](internal/stringToArray.md)

[《lodash源码分析之values》](values.md)

## 源码分析

`toArray` 的作用是将 `value` 转换成数组。

### 一些常量

```javascript
const mapTag = '[object Map]'
const setTag = '[object Set]'

const symIterator = Symbol.iterator
```

将 `Map` 和 `Set` 的 `Tag` 保存，方便后续使用。

也将 `Symbol.iterator` 保存，优化性能。

### 假值

```javascript
if (!value) {
  return []
}
```

如果传入的 `value` 为假值，则返回一个空数组。

### 类数组

类数组包括字符串和其它类数组，其中字符串是分开处理的。

```javascript
if (isArrayLike(value)) {
  return isString(value) ? stringToArray(value) : copyArray(value)
}
```

可以看到，如果是字符串，则使用 `stringToArray` 来转换。

如果是其它的类数组，则使用 `copyArray` 将原来的数组复制一次即可。

### 迭代器

```javascript
if (symIterator && value[symIterator]) {
  return iteratorToArray(value[symIterator]())
}
```

如果在支持迭代器的环境中，并且 `value` 中又有迭代器，则使用 `iteratorToArray` 转换。

### 其它类型

```javascript
const tag = getTag(value)
const func = tag == mapTag ? mapToArray : (tag == setTag ? setToArray : values)

return func(value)
```

通过 `getTag` 函数获取到 `value` 的 `tag` 。

如果是 `Map` 类型，则使用 `mapToArray` 转换。

如果是 `Set` 类型，则使用 `setToArray` 转换。

其它类型，都作为对象看待，使用 `values` 方法转换。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 