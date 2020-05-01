# lodash源码分析之baseIsEqualDeep

本文为读 lodash 源码的第二百一十八篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import Stack from './Stack.js'
import equalArrays from './equalArrays.js'
import equalByTag from './equalByTag.js'
import equalObjects from './equalObjects.js'
import getTag from './getTag.js'
import isBuffer from '../isBuffer.js'
import isTypedArray from '../isTypedArray.js'
```

[《lodash源码分析之Stack》](./Stack.md)
[《lodash源码分析之equalArrays》](./equalArrays.md)
[《lodash源码分析之equalByTag》](./equalByTag.md)
[《lodash源码分析之equalObjects》](./equalObjects.md)
[《lodash源码分析之getTag》](./getTag.md)
[《lodash源码分析之isBuffer》](../isBuffer.md)
[《lodash源码分析之isTypedArray》](../isTypedArray.md)

## 源码分析

`baseIsEqualDeep` 的作用是深度比较 `object` 和 `other` ，看这两个值是否相等。

### 一些变量

```javascript
const COMPARE_PARTIAL_FLAG = 1

const argsTag = '[object Arguments]'
const arrayTag = '[object Array]'
const objectTag = '[object Object]'

const hasOwnProperty = Object.prototype.hasOwnProperty
```

`COMPARE_PARTIAL_FLAG` 用来保存是否局部比较的标识。

`argsTag` 用来保存 `arguments` 对象通过 `Object.prototype.toString.call` 后得到的结果，这里统称为 `tag`。

`arrayTag` 用来保存 `Array` 对象的 `tag`。

`objectTag` 用来保存 `Object` 对象的 `tag` 。

`hasOwnProperty` 用来保存 `Object.prototype.hasOwnProperty` ，主要是方便调用来优化性能。

### 处理arrayTag

```javascript
let objIsArr = Array.isArray(object)
const othIsArr = Array.isArray(other)
let objTag = objIsArr ? arrayTag : getTag(object)
let othTag = othIsArr ? arrayTag : getTag(other)
```

后面 `Array` 是分开处理的，这里使用 `Array.isArray` 来判断是否为数组，没有直接使用 `getTag` 方法，是因为 `Array.isArray` 性能会更加好。

### 处理argsTag

```javascript
objTag = objTag == argsTag ? objectTag : objTag
othTag = othTag == argsTag ? objectTag : othTag
```

可以看到，`argsTag` 会统一成 `objectTag` ，也即后续 `arguments` 对象会跟对象一样，使用同样的比较方式。

### 处理Tag相同的情况

#### 判断Tag是否相同

```javascript
let objIsObj = objTag == objectTag
const othIsObj = othTag == objectTag
const isSameTag = objTag == othTag
```

使用 `objTag == othTag` 即可判断 `Tag` 是否相同。

#### 处理Buffer

```javascript
if (isSameTag && isBuffer(object)) {
  if (!isBuffer(other)) {
    return false
  }
  objIsArr = true
  objIsObj = false
}
```

如果 `object` 是 `Buffer` 类型，而 `other` 又不是 `Buffer` 类型，则两者肯定不相等，直接返回 `false` 。

否则将 `objIsArr` 的标志位置为 `true`，`objIsObj` 的标志位置为 `false` 。也即将 `object` 标记为数组。

#### 处理非对象类型

```javascript
if (isSameTag && !objIsObj) {
  stack || (stack = new Stack)
  return (objIsArr || isTypedArray(object))
    ? equalArrays(object, other, bitmask, customizer, equalFunc, stack)
  : equalByTag(object, other, objTag, bitmask, customizer, equalFunc, stack)
}
```

在 `eqaulArrays` 和 `equalByTag` 的相关文章中，我们知道，`stack` 是拿来处理循环引用比较的，它是在这里初始化的。

上面处理 `Buffer` 的时候，如果 `object` 和 `other` 都为 `Buffer` 的时候， `objeIsObj` 的值会被设置为 `false`，也即 `Buffer` 的处理会落到这个分支。

接着判断，如果 `objIsArr` 也即为数组或 `Buffer` ，或者为 `TypedArray` 类型，都调用 `equalArrays` 方法来判断两者是否相等。

这也很容易理解，`Buffer` 和 `TypedArray` 其实就是字节数组，完全可以使用数组的比较方式。

至于其它类型，则使用 `equalByTag` 来比较，`equalByTag` 方法，会根据 `Tag` 的不同，选择不同的比较方式。

### 处理lodash包装对象

```javascript
if (!(bitmask & COMPARE_PARTIAL_FLAG)) {
  const objIsWrapped = objIsObj && hasOwnProperty.call(object, '__wrapped__')
  const othIsWrapped = othIsObj && hasOwnProperty.call(other, '__wrapped__')

  if (objIsWrapped || othIsWrapped) {
    const objUnwrapped = objIsWrapped ? object.value() : object
    const othUnwrapped = othIsWrapped ? other.value() : other

    stack || (stack = new Stack)
    return equalFunc(objUnwrapped, othUnwrapped, bitmask, customizer, stack)
  }
}
```

包装对象是 `lodash` 所定义的一种数据格式，会有 `__wrapped__` 属性，这种对象会延迟计算，只有在显示调用 `value` 方法时才会计算值。

因此遇到这种对象，会调用 `value` 方法得到真正的值，再使用 `equalFunc` 进行比较。

### 处理对象

```javascript
if (!isSameTag) {
  return false
}
stack || (stack = new Stack)
return equalObjects(object, other, bitmask, customizer, equalFunc, stack)
```

如果两个值的 `Tag` 不一样，因为类型都不一样，两个值肯定不相等，直接返回 `false` 。

最终最终，来到了对象的比较，调用 `equalObjects` 函数进行对象比较即可。


## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 