# lodash源码分析之size

本文为读 lodash 源码的第一百七十篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import getTag from './.internal/getTag.js'
import isArrayLike from './isArrayLike.js'
import isString from './isString.js'
import stringSize from './.internal/stringSize.js'
```

[《lodash源码分析之getTag》](internal/getTag.md)
[《lodash源码分析之isArrayLike》](isArrayLike.md)
[《lodash源码分析之isString》](isString.md)
[《lodash源码分析之stringSize》](internal/stringSize.md)

## 源码分析

`size` 用来返回类数组的长度、`Map` 或 `Set` 的大小，或者对象自身可枚举属性的数量。

源码如下：

```javascript
const mapTag = '[object Map]'
const setTag = '[object Set]'

function size(collection) {
  if (collection == null) {
    return 0
  }
  if (isArrayLike(collection)) {
    return isString(collection) ? stringSize(collection) : collection.length
  }
  const tag = getTag(collection)
  if (tag == mapTag || tag == setTag) {
    return collection.size
  }
  return Object.keys(collection).length
}
```

### 处理 `null` 和 `undefined`

如果 `collection == null` ，即传入的 `collection` 为 `null` 或者 `undefined`，直接返回 `0` 。

### 处理类数组

以下代码为处理类数组的代码：

```javascript
if (isArrayLike(collection)) {
  return isString(collection) ? stringSize(collection) : collection.length
}
```

可以看到，类数组的处理也分两种情况，因为 `string` 其实也是类数组的一种，如果 `collection` 为 `string` 类型，则使用 `stringSize` 来获取字符串的长度，否则直接通过 `length` 属性获取。

### 处理 `Map` 和 `Set`

以下为处理 `Map` 和 `Set` 的代码：

```javascript
const mapTag = '[object Map]'
const setTag = '[object Set]'

const tag = getTag(collection)
if (tag == mapTag || tag == setTag) {
  return collection.size
}
```

通过 `getTag` 来判断传入的 `collection` 是否为 `Map` 或者 `Set` 类型，如果是，则直接取其 `size` 属性的值。

### 处理对象

以下为处理对象的代码：
```javascript
return Object.keys(collection).length
```

`Object.keys` 可以获取到对象所有自身可枚举的属性，取其长度即可。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 