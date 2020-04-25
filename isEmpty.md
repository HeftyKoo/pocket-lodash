# lodash源码分析之isEmpty

本文为读 lodash 源码的第二百一十二篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import getTag from './.internal/getTag.js'
import isArguments from './isArguments.js'
import isArrayLike from './isArrayLike.js'
import isBuffer from './isBuffer.js'
import isPrototype from './.internal/isPrototype.js'
import isTypedArray from './isTypedArray.js'
```

《[lodash源码分析之数据类型获取的兼容性](internal/getTag.md)》
《[lodash源码分析之isArguments](isArguments.md)》
《[lodash源码分析之isArrayLike](isArrayLike.md)》
《[lodash源码分析之isBuffer](isBuffer.md)》
《[lodash源码分析之isPrototype](internal/isPrototype.md)》
《[lodash源码分析之isTypedArray](isTypedArray.md)》

## 源码分析

`isEmpty` 用来判断 `value` 是否为空值。

源码如下：

```javascript
const hasOwnProperty = Object.prototype.hasOwnProperty
function isEmpty(value) {
  if (value == null) {
    return true
  }
  if (isArrayLike(value) &&
      (Array.isArray(value) || typeof value === 'string' || typeof value.splice === 'function' ||
        isBuffer(value) || isTypedArray(value) || isArguments(value))) {
    return !value.length
  }
  const tag = getTag(value)
  if (tag == '[object Map]' || tag == '[object Set]') {
    return !value.size
  }
  if (isPrototype(value)) {
    return !Object.keys(value).length
  }
  for (const key in value) {
    if (hasOwnProperty.call(value, key)) {
      return false
    }
  }
  return true
}
```

### null 及 undefined

```javascript
if (value == null) {
  return true
}
```

如果 `null` 是 `undefined` ，则返回 `true`。

### 类数组对象

类数组对象有很多，如果是类数组对象，则通过 `length` 来判断集合是否为空。

其中 `Array.isArray` 用来判断是否为数组；

`typeof value === 'string'` 用来判断是否为字符串；

`typeof value.splice === 'function'` 用来判断是否为 `jquery-like` 对象，`jquery-like` 对象会有这个 `splice` 方法；

`isBuffer` 用来判断是否为 `Buffer` ；

`isTypedArray` 用来判断是否为 `TypedArray` ；

`isArguments` 用来判断是否为 `arguments` 对象；

对于以上类型，都用 `length` 属性来判断是否为空。

### Map及Set

```javascript
 const tag = getTag(value)
 if (tag == '[object Map]' || tag == '[object Set]') {
   return !value.size
 }
```

如果是 `Map` 及 `Set` ，则用 `size` 来判断是否为空。

### 原型对象

```javascript
if (isPrototype(value)) {
  return !Object.keys(value).length
}
```

如果为原型对象，则判断自身可枚举属性是否为空。

### 一般性对象及其它类型

```javascript
for (const key in value) {
  if (hasOwnProperty.call(value, key)) {
    return false
  }
}
```

`for...in` 会遍历自身及原型链上的可枚举属性。

这里配合 `Object.prototype.hasOwnProperty` 使用表示只遍历自身可枚举属性。

咋一看，这不是和 `Object.keys` 的作用一样吗，为什么不直接和原型一样，使用 `Object.keys` 来判断呢，而且 `Object.keys` 的性能更好。

其实是因为 `Object.keys` 会对原始值抛出一个 `TypeError` 的错误，但是 `for...in` 不会抛出错误。这里使用 `for...in` 是为了避免错误的发生。

看 `MDN` 上 `Object.keys` 的 `polyfill` 就很清楚两者的区别。

```javascript
if (!Object.keys) {
  Object.keys = (function () {
    var hasOwnProperty = Object.prototype.hasOwnProperty,
        hasDontEnumBug = !({toString: null}).propertyIsEnumerable('toString'),
        dontEnums = [
          'toString',
          'toLocaleString',
          'valueOf',
          'hasOwnProperty',
          'isPrototypeOf',
          'propertyIsEnumerable',
          'constructor'
        ],
        dontEnumsLength = dontEnums.length;

    return function (obj) {
      if (typeof obj !== 'object' && typeof obj !== 'function' || obj === null) throw new TypeError('Object.keys called on non-object');

      var result = [];

      for (var prop in obj) {
        if (hasOwnProperty.call(obj, prop)) result.push(prop);
      }

      if (hasDontEnumBug) {
        for (var i=0; i < dontEnumsLength; i++) {
          if (hasOwnProperty.call(obj, dontEnums[i])) result.push(dontEnums[i]);
        }
      }
      return result;
    }
  })()
};
```

这个 `polyfill` 会有一些兼容性的代码。

## 参考资料

[MDN: Object.keys()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/keys)

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 