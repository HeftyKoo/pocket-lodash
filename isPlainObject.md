# lodash源码分析之isPlainObject

本文为读 lodash 源码的第二百一十篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import getTag from './.internal/getTag.js'
import isObjectLike from './isObjectLike.js'
```

《[lodash源码分析之数据类型获取的兼容性](./internal/getTag.md)》

《[lodash源码分析之isObjectLike](isObjectLike.md)》

## 源码分析

`isPlainObject` 用来检测 `value` 值是否为纯对象。

纯对象是指，`value` 是由 `Object` 构造函数创建的，也即原型为 `Object.prototype` ，或者原型为 `null` 的对象。

源码如下：

```javascript
function isPlainObject(value) {
  if (!isObjectLike(value) || getTag(value) != '[object Object]') {
    return false
  }
  if (Object.getPrototypeOf(value) === null) {
    return true
  }
  let proto = value
  while (Object.getPrototypeOf(proto) !== null) {
    proto = Object.getPrototypeOf(proto)
  }
  return Object.getPrototypeOf(value) === proto
}
```

### 判断是否为对象

```javascript
if (!isObjectLike(value) || getTag(value) != '[object Object]') {
  return false
}
```

先使用 `isObjectLike` 来判断 `value` 是否为类对象类型，如果不是，返回 `false`。

再用 `getTag` 来获取类型标签，通过类型标签再排除掉除了类对象类型。

这里需要注意，如果通过 `Symbol.toStringTag` 更改了 `tag` ，也会返回 `false`。

例如：

```javascript
var object = {};
Object.defineProperty(object, Symbol.toStringTag, {
  'configurable': true,
  'enumerable': false,
  'writable': false,
  'value': 'Test'
})
```

通过 `getTag` 获取到的会是 `[object Test]` ，通不过检测。

### 原型为null的对象

```javascript
if (Object.getPrototypeOf(value) === null) {
  return true
}
```

使用 `Object.getPrototypeOf(value)` 来获取 `value` 的原型，如果原型为 `null` ，则也是纯对象。

### 原型链是否只有一层

```javascript
let proto = value
while (Object.getPrototypeOf(proto) !== null) {
  proto = Object.getPrototypeOf(proto)
}
return Object.getPrototypeOf(value) === proto
```

使用 `while` 循环遍历 `value` 的原型链，然后用 `Object.getPrototypeOf` 获取 `value` 的原型，如果最顶层的原型和 `value` 的原型一致，则表示原型链只有一层，也为纯对象。

这里为什么不直接用 `Object.getPrototypeOf(value) === Object.prototype` 呢？

因为可能会在其他的窗口对 `value` 进行比较，例如在父级容器对 `iframe` 中的对象进行判断，这时 `Object.prototype` 是父级的 `Object`，并不是 `iframe` 中的 `Object` ，直接这样比较，可能会和结果有偏差。


## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 