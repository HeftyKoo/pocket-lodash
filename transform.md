# lodash源码分析之transform

本文为读 lodash 源码的第三百零四篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import arrayEach from './.internal/arrayEach.js'
import baseForOwn from './.internal/baseForOwn.js'
import isBuffer from './isBuffer.js'
import isObject from './isObject.js'
import isTypedArray from './isTypedArray.js'
```

[lodash源码分析之arrayEach](./internal/arrayEach.md)

[lodash源码分析之baseForOwn](./internal/baseForOwn.md)

[lodash源码分析之isBuffer](./isBuffer.md)

[lodash源码分析之isObject](./isObject.md)

[lodash源码分析之isTypedArray](./isTypedArray.md)

## 源码分析

`transform` 的作用跟 `reduce` 差不多，会将 `object` 转换成一个新的 `accumulator` 对象，如果 `accumulator` 没有传入时，会创建一个和 `object` 的 `[[Prototype]]` 相同的新对象作为初始值，即两者的原型相同。

源码如下：

```javascript
function transform(object, iteratee, accumulator) {
  const isArr = Array.isArray(object)
  const isArrLike = isArr || isBuffer(object) || isTypedArray(object)

  if (accumulator == null) {
    const Ctor = object && object.constructor
    if (isArrLike) {
      accumulator = isArr ? new Ctor : []
    }
    else if (isObject(object)) {
      accumulator = typeof Ctor === 'function'
        ? Object.create(Object.getPrototypeOf(object))
        : {}
    }
    else {
      accumulator = {}
    }
  }
  (isArrLike ? arrayEach : baseForOwn)(object, (value, index, object) =>
    iteratee(accumulator, value, index, object))
  return accumulator
}
```

### 处理类数组

```javascript
if (isArrLike) {
  accumulator = isArr ? new Ctor : []
}
```

如果是数组，则直接调用 `new Ctro` ，即直接使用原来的构造函数，否则，初始化一个空数组 `[]` 。

### 处理对象

```javascript
else if (isObject(object)) {
  accumulator = typeof Ctor === 'function'
    ? Object.create(Object.getPrototypeOf(object))
  : {}
} else {
  accumulator = {}
}
```

如果 `Ctro` 为函数，即存在构造函数，则调用 `Object.create` 来得到一个和原 `object` 同样原型链的新对象。

否则直接使用空对象。

其它情况也使用新对象。

### 迭代

```javascript
(isArrLike ? arrayEach : baseForOwn)(object, (value, index, object) =>
    iteratee(accumulator, value, index, object))
```

接下来，就根据 `object` 的类型来选择不同的遍历函数，在遍历的过程中，调用 `iteratee` 迭代器，传入 `accumulator` 和 `value` 、`index` 及原 `object` ，可以在 `iteratee` 里直接修改 `accumulator` 。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 