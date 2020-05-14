# lodash源码分析之baseMergeDeep

本文为读 lodash 源码的第二百七十八篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import assignMergeValue from './assignMergeValue.js'
import cloneBuffer from './cloneBuffer.js'
import cloneTypedArray from './cloneTypedArray.js'
import copyArray from './copyArray.js'
import initCloneObject from './initCloneObject.js'
import isArguments from '../isArguments.js'
import isArrayLikeObject from '../isArrayLikeObject.js'
import isBuffer from '../isBuffer.js'
import isObject from '../isObject.js'
import isPlainObject from '../isPlainObject.js'
import isTypedArray from '../isTypedArray.js'
import toPlainObject from '../toPlainObject.js'
```

[lodash源码分析之assignMergeValue](./assignMergeValue.md)

[lodash源码分析之cloneBuffer](./cloneBuffer.md)

[lodash源码分析之cloneTypedArray](./cloneTypedArray.md)

[lodash源码分析之copyArray](./copyArray.md)

[lodash源码分析之initCloneObject](./initCloneObject.md)

[lodash源码分析之isArguments](../isArguments.md)

[lodash源码分析之isArrayLikeObject](../isArrayLikeObject.md)

[lodash源码分析之isBuffer](../isBuffer.md)

[lodash源码分析之isObject](../isObject.md)

[lodash源码分析之isPlainObject](../isPlainObject.md)

[lodash源码分析之isTypedArray](../isTypedArray.md)

[lodash源码分析之toPlainObject](../toPlainObject.md)

## 源码分析

`baseMergeDeep` 是用来对数组和对齐进行深度 `merge` 的内部方法。

### 参数说明

* `object` ：目标对象，源对象的属性值会合并到这个对象上。
* `source` ：源对象
* `key`：需要合并的属性
* `srcIndex`： `source` 的索引
* `mergeFunc` ：合并值的方法，也正是这个方法产生递归
* `customizer`： 自定义合并方法
* `stack` ：`Stack` 实例，用来记录循环引用

### 循环引用处理

```javascript
const objValue = object[key]
const srcValue = source[key]
const stacked = stack.get(srcValue)

if (stacked) {
  assignMergeValue(object, key, stacked)
  return
}
```

如果 `srcValue` 已经在 `stack` 中有记录，表示当前正处于循环引用中，直接调用 `assignMergeValue` 将值 `stacked` 合并值即可。

循环引用的处理和 [《lodash源码分析之equalArrays》](./equalArrays.md) 的处理类似。

### 自定义合并函数

```javascript
let newValue = customizer
? customizer(objValue, srcValue, `${key}`, object, source, stack)
: undefined
```

如果有自定义 `customizer` 函数，则直接调用 `customizer` 函数得到合并后的值 `newValue` 。

### 整体流程

```javascript
let isCommon = newValue === undefined

  if (isCommon) {
    。。。。
    这块逻辑会生成newValue和更新isCommon标识
  }
  if (isCommon) {
    stack.set(srcValue, newValue)
    mergeFunc(newValue, srcValue, srcIndex, customizer, stack)
    stack['delete'](srcValue)
  }
  assignMergeValue(object, key, newValue)
```

如果不管其它逻辑，单看最后一句，其实就是调用 `assignMergeValue` 来更新合并后的值 `newValue` 。

因此可以知道，`isCommon` 其实就是还需不需要处理 `newValue` 的标识。

因此如果自定义函数 `customizer` 返回的值是 `undefined` 或者没有自定义函数，就需要经过一翻处理，才能得到合并后的值 `newValue` 。

如果经过这轮处理后，得到的 `newValue` 不需要继续处理，则 `isCommon` 会被置为 `false` ，如果还要继续处理，就会走 `mergeFunc` 的逻辑，在 `mergeFunc` 这里会产生递归，也即达到了深度 `merge` 的目的。

### 处理数组类型

```javascript
if (isCommon) {
  const isArr = Array.isArray(srcValue)
  const isBuff = !isArr && isBuffer(srcValue)
  const isTyped = !isArr && !isBuff && isTypedArray(srcValue)

  newValue = srcValue
  if (isArr || isBuff || isTyped) {
    if (Array.isArray(objValue)) {
      newValue = objValue
    }
    else if (isArrayLikeObject(objValue)) {
      newValue = copyArray(objValue)
    }
    else if (isBuff) {
      isCommon = false
      newValue = cloneBuffer(srcValue, true)
    }
    else if (isTyped) {
      isCommon = false
      newValue = cloneTypedArray(srcValue, true)
    }
    else {
      newValue = []
    }
  }
}
```

#### 数组

```javascript
if (Array.isArray(objValue)) {
  newValue = objValue
}
```

如果 `objValue` 为数组，则将 `newValue` 指向 `objValue` ，可以看到，在数组时，`objValue` 和 `srcValue` 并没有进行任何合并的操作，因为作为数组来说，肯定是需要深度合并的，因此这里也没有将 `isCommon` 设置为 `false` 。

#### 类数组

```javascript
else if (isArrayLikeObject(objValue)) {
  newValue = copyArray(objValue)
}
```

因为 `srcValue` 为数组，但是 `objValue` 却为类数组，类型不匹配，因此要调用 `copyArray` 将 `objValue` 中的值复制成一个数组，这就变成了数组的深度合并。

#### Buffer

```javascript
else if (isBuff) {
  isCommon = false
  newValue = cloneBuffer(srcValue, true)
}
```

`Buffer` 其实可以看作是由`0` 和 `1` 组成的一维数组，因为和原来的值可能是不同的内存，合并的时候直接调用 `cloneBuffer` 将 `srcValue` 进行复制，然后覆盖 `objValue` 的值即可。

这时得到的 `newValue` 就是最终的结果了，不需要再进行递归合并，将 `isCommon` 设置为 `false` 。

#### TypedArray

```javascript
else if (isTyped) {
  isCommon = false
  newValue = cloneTypedArray(srcValue, true)
}
```

`TypedArray` 和 `Buffer` 类似， `TypedArray` 是 `Buffer` 的视图，处理方式和 `Buffer` 一样。

#### 其它情况

```javascript
else {
  newValue = []
}
```

其它情况和 `srcValue` 的值都不兼容，直接将 `newValue` 初始化成空数组，其实就是将 `objValue` 直接丢弃掉了。

但是因为 `srcValue` 是数组类型，也是需要递归将值复制过来的。

### 对象处理

```javascript
else if (isPlainObject(srcValue) || isArguments(srcValue)) {
  newValue = objValue
  if (isArguments(objValue)) {
    newValue = toPlainObject(objValue)
  }
  else if (typeof objValue === 'function' || !isObject(objValue)) {
    newValue = initCloneObject(srcValue)
  }
}
  else {
    isCommon = false
  }
```

如果是 `objValue` 如果是 `arguments` 对象，则将 `objValue` 转换成纯对象，然后再递归合并。

如果 `objValue` 为函数，或者非对象类型，则调用 `initCloneObject` 来初始化一个空对象，其实也就将 `objValue` 原来的值丢弃了，因为 `srcValue` 是对象，因此也需要递归合并。

其它情况 `isCommon` 都为 `false` ，即可以直接调用 `assignMergeValue` 合并即可。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 