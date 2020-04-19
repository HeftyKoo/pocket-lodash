# lodash源码分析之baseClone

本文为读 lodash 源码的第一百九十八篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import Stack from './Stack.js'
import arrayEach from './arrayEach.js'
import assignValue from './assignValue.js'
import cloneBuffer from './cloneBuffer.js'
import copyArray from './copyArray.js'
import copyObject from './copyObject.js'
import cloneArrayBuffer from './cloneArrayBuffer.js'
import cloneDataView from './cloneDataView.js'
import cloneRegExp from './cloneRegExp.js'
import cloneSymbol from './cloneSymbol.js'
import cloneTypedArray from './cloneTypedArray.js'
import copySymbols from './copySymbols.js'
import copySymbolsIn from './copySymbolsIn.js'
import getAllKeys from './getAllKeys.js'
import getAllKeysIn from './getAllKeysIn.js'
import getTag from './getTag.js'
import initCloneObject from './initCloneObject.js'
import isBuffer from '../isBuffer.js'
import isObject from '../isObject.js'
import isTypedArray from '../isTypedArray.js'
import keys from '../keys.js'
import keysIn from '../keysIn.js'
```

[《lodash源码分析之Stack》](./Stack.md)
[《lodash源码分析之arrayEach》](./arrayEach.md)
[《lodash源码分析之assignValue》](./assignValue.md)
[《lodash源码分析之cloneBuffer》](./cloneBuffer.md)
[《lodash源码分析之copyArray》](./copyArray.md)
[《lodash源码分析之copyObject》](./copyObject.md)
[《lodash源码分析之cloneArrayBuffer》](./cloneArrayBuffer.md)
[《lodash源码分析之cloneDataView》](./cloneDataView.md)
[《lodash源码分析之cloneRegExp》](./cloneRegExp.md)
[《lodash源码分析之cloneSymbol》](./cloneSymbol.md)
[《lodash源码分析之cloneTypedArray》](./cloneTypedArray.md)
[《lodash源码分析之copySymbols》](./copySymbols.md)
[《lodash源码分析之copySymbolsIn》](./copySymbolsIn.md)
[《lodash源码分析之getAllKeys》](./getAllKeys.md)
[《lodash源码分析之getAllKeysIn》](./getAllKeysIn.md)
[《lodash源码分析之getTag》](./getTag.md)
[《lodash源码分析之initCloneObject》](./initCloneObject.md)
[《lodash源码分析之isBuffer》](../isBuffer.md)
[《lodash源码分析之isObject》](../isObject.md)
[《lodash源码分析之isTypedArray》](../isTypedArray.md)
[《lodash源码分析之keys》](../keys.md)
[《lodash源码分析之keysIn》](../keysIn.md)

## 源码分析

`baseClone` 是实现 `clone` 、 `cloneDeep` 等一系列复制函数的内部函数，也是这些复制函数的核心。

### 前期准备

#### 逻辑运算符

```javascript
const CLONE_DEEP_FLAG = 1
const CLONE_FLAT_FLAG = 2
const CLONE_SYMBOLS_FLAG = 4

function baseClone (value, bitmask, customizer, key, object, stack) {
  const isDeep = bitmask & CLONE_DEEP_FLAG
  const isFlat = bitmask & CLONE_FLAT_FLAG
  const isFull = bitmask & CLONE_SYMBOLS_FLAG
  ...
}
```

源码的开始就定义了 `CLONE_DEEP_FLAG`、`CLONE_FLAT_FLAG` 和 `CLONE_SYMBOLS_FLAG` 几个变量，并且在 `baseClone` 函数的开始就和 `bitmask` 来做逻辑与，也就是二进制运算，得到 `isDeep` 、`isFlat` 和 `isFull` 几个标志位。

`isDeep` 用来标记是否需要深拷贝，`isFlat` 用来标记是否需要拷贝原型链上的属性，`isFull` 用来标记是否需要拷贝 `Symbol` 类型的属性。

这里为什么要用到二进制运算呢？二进制运算在这里最大的好处是可以节约参数。

可以看到，这三个状态位是没有关联的，任何一个状态位的开关都和其他状态位无关，如果用布尔值来控制，则至需要三个参数。但是用二进制与运算只需要一个参数即可。

来看看是如何做到的。

先来看看这三个 `FLAG` 的二进制码是多少：

```javascript
const CLONE_DEEP_FLAG = 1 // 001
const CLONE_FLAT_FLAG = 2 // 010
const CLONE_SYMBOLS_FLAG = 4 // 100
```

因此，如果我需要开启 `isDeep` ，只需要 `bitmask` 的第一位为 `1` 即可，最简单就是传入 `001` ，即和 `CLONE_DEEP_FLAG` 的值相同。

同理，如果要开启 `isFlat` ，第二位为 `1` 即可，最简单也是传入 `CLONE_FLAT_FLAG` 即可。

我们又知道，二进制的与运算，只要相同位有一个为 `1`，则该位在与运算后改定为 `1` 。

因此如果要同时开启 `isDeep` 和 `isFlat` ，`bitmask` 只需要传入 `CLONE_DEEP_FLAG | CLONE_FLAT_FLAG` ，因为这保证了第一位和第二位都为 `1` ，`isDeep` 和 `isFlat` 必定为真值。

因此，这三个状态位的开启关闭组合，完全可以通过三个 `FLAG` 的或组合来完成，通过一个参数 `bitmask` 传入，节约了两个参数。

####可复制对象及不可复制对象
```javascript
/** `Object#toString` result references. */
const argsTag = '[object Arguments]'
const arrayTag = '[object Array]'
const boolTag = '[object Boolean]'
const dateTag = '[object Date]'
const errorTag = '[object Error]'
const mapTag = '[object Map]'
const numberTag = '[object Number]'
const objectTag = '[object Object]'
const regexpTag = '[object RegExp]'
const setTag = '[object Set]'
const stringTag = '[object String]'
const symbolTag = '[object Symbol]'
const weakMapTag = '[object WeakMap]'

const arrayBufferTag = '[object ArrayBuffer]'
const dataViewTag = '[object DataView]'
const float32Tag = '[object Float32Array]'
const float64Tag = '[object Float64Array]'
const int8Tag = '[object Int8Array]'
const int16Tag = '[object Int16Array]'
const int32Tag = '[object Int32Array]'
const uint8Tag = '[object Uint8Array]'
const uint8ClampedTag = '[object Uint8ClampedArray]'
const uint16Tag = '[object Uint16Array]'
const uint32Tag = '[object Uint32Array]'

/** Used to identify `toStringTag` values supported by `clone`. */
const cloneableTags = {}
cloneableTags[argsTag] = cloneableTags[arrayTag] =
cloneableTags[arrayBufferTag] = cloneableTags[dataViewTag] =
cloneableTags[boolTag] = cloneableTags[dateTag] =
cloneableTags[float32Tag] = cloneableTags[float64Tag] =
cloneableTags[int8Tag] = cloneableTags[int16Tag] =
cloneableTags[int32Tag] = cloneableTags[mapTag] =
cloneableTags[numberTag] = cloneableTags[objectTag] =
cloneableTags[regexpTag] = cloneableTags[setTag] =
cloneableTags[stringTag] = cloneableTags[symbolTag] =
cloneableTags[uint8Tag] = cloneableTags[uint8ClampedTag] =
cloneableTags[uint16Tag] = cloneableTags[uint32Tag] = true
cloneableTags[errorTag] = cloneableTags[weakMapTag] = false
```

这里是初始化可复制对象及不可复制对象的标识字典。

后续会调用 `getTag` 获取对象 `Object.prototype.toString` 后的值，用来识别对象的类型。

可以看到，`Error` 和 `WeakMap` 类型都是不可复制的。其实除了这两个外，`WeakSet` ，`DOM` 对象都是不可以复制的，因为这些类型没有标记为 `true` 。

对于不可以复制的对象，会直接使用空对象来替换。

#### initCloneByTag

通过 `Object.prototype.toString.call` 获取到对象的 `tag` ，针对 `tag` 来判断不同的对象类型，来初始化复制的数据。

代码如下：

```javascript
function initCloneByTag(object, tag, isDeep) {
  const Ctor = object.constructor
  switch (tag) {
    case arrayBufferTag:
      return cloneArrayBuffer(object)

    case boolTag:
    case dateTag:
      return new Ctor(+object)

    case dataViewTag:
      return cloneDataView(object, isDeep)

    case float32Tag: case float64Tag:
    case int8Tag: case int16Tag: case int32Tag:
    case uint8Tag: case uint8ClampedTag: case uint16Tag: case uint32Tag:
      return cloneTypedArray(object, isDeep)

    case mapTag:
      return new Ctor

    case numberTag:
    case stringTag:
      return new Ctor(object)

    case regexpTag:
      return cloneRegExp(object)

    case setTag:
      return new Ctor

    case symbolTag:
      return cloneSymbol(object)
  }
}
```

这段代码的逻辑很简单，对于马上可以进行复制的数据，会马上进行复制，对于需要有特殊逻辑处理的数据，会先创建一个新的实例，后续再进行数据复制。

#### 数组复制初始化

一般的数组在初始化时，直接使用 `new Array(length)` 初始化一个空数组即可。

但是如果使用正则的 `exec` 方法时，返回的结果也是一个数组，但是这个数组有两个比较特殊的属性，一个是 `input` ，保存原始的字符串，一个是 `index` ，保存上一次匹配文本的第一个字符的位置，因此也需要对这两个属性进行复制。

源码如下：

```javascript
function initCloneArray(array) {
  const { length } = array
  const result = new array.constructor(length)

  if (length && typeof array[0] === 'string' && hasOwnProperty.call(array, 'index')) {
    result.index = array.index
    result.input = array.input
  }
  return result
}
```

`new array.constructor(length)` 其实相当于 `new Array(length)` 。

接下来这段是判断传入的 `array` 是否为正则 `exec` 方法所返回的结果。

```javascript
length && typeof array[0] === 'string' && hasOwnProperty.call(array, 'index')
```

如果是正则 `exec` 方法返回的结果，数组的第一个值肯定为匹配出来的字符串，并且自身有 `index` 属性。

如果是这种类型的数组，在初始化的时候，将 `index` 和 `input` 属性进行复制。

### 参数说明

#### value

要复制的 `value` 值

#### bitmask

控制 `isDeep` 、`isFlat` 和 `isFull` 标记的二进制值。

#### customizer

自定义值复制函数，如果传入了这个函数 ，则复制值的时候会直接使用这个函数复制，不使用 `baseClone` 里的复制逻辑。

#### key

传入 `value` 对应的属性 `key` 。

#### object

当前 `value` 所属的父级对象。

#### stack

`Stack` 类的实例，用来存储引用类的 `value` ，会用来避免循环依赖。

### 自定义复制函数

这是 `baseClone` 最先处理的复制逻辑，相关代码如下：

```javascript
let result
if (customizer) {
  result = object ? customizer(value, key, object, stack) : customizer(value)
}
if (result !== undefined) {
  return result
}
```

有传 `customizer` 自定义复制函数参数时，`baseClone` 直接调用传入的 `customizer` 函数，得到结果 `result` ，因为复制逻辑已经由 `customizer` 处理了，`baseClone` 已经不需要额外的处理，直接将结果 `result` 返回即可。

这里有个需要注意的地方，如果没有传 `value` 父级 `object` 时，那 `key` 也没有意义，因此直接传入 `value` 即可，

### 简单数据类型复制

```javascript
if (!isObject(value)) {
  return value
}
```

对于简单的数据类型，因为不涉及到内存引用相同的问题，直接将同样的值返回即可。

### 数组浅复制

数组浅复制相关的源码如下：

```javascript
const isArr = Array.isArray(value)
const tag = getTag(value)
if (isArr) {
  result = initCloneArray(value)
  if (!isDeep) {
    return copyArray(value, result)
  }
} else {
  ...
}
```

使用 `Array.isArray` 来判断 `value` 是否为数组，如果是数组，则用 `initCloneArray` 来初始化数组容器。

如果 `isDeep` 为假值，则使用 `copyArray` 将 `value` 中每一项都复制到 `result` 中，`copyArray` 会将 `result` 返回。

### Buffer复制

`Buffer` 复制相关的源码如下：

```javascript
if (isArr) {
  ...
} else {
  const isFunc = typeof value === 'function'

  if (isBuffer(value)) {
    return cloneBuffer(value, isDeep)
  }
}
```

使用 `isBuffer` 函数判断 `value` 是否为 `Buffer` 类型，如果是 `Buffer` 类型，则无论是否为浅复制还是深度复制，都调用 `cloneBuffer` 复制 `value` 。

### Object、Arguments、Function 浅复制

```javascript
if (isArr) {
  ...
} else {
  ...
  if (tag == objectTag || tag == argsTag || (isFunc && !object)) {
    result = (isFlat || isFunc) ? {} : initCloneObject(value)
    if (!isDeep) {
      return isFlat
        ? copySymbolsIn(value, copyObject(value, keysIn(value), result))
      : copySymbols(value, Object.assign(result, value))
    }
  } else {
    if (isFunc || !cloneableTags[tag]) {
      return object ? value : {}
    }
    result = initCloneByTag(value, tag, isDeep)
  }
}
```

先来看这个判断条件:

```javascript
tag == objectTag || tag == argsTag || (isFunc && !object)
```

前面两个条件分别是判断是否为 `Object` 和 `Arguments` ，`isFunc && !Object` 的意思是只传入了一个 `function` ，即类似于这样的调用方式：
```javascript
baseClone(function () {})
```

在 `isFlat` 为真值，或者 `isFunc` 为 `true` ，**也即传入的 `value` 为函数，但是没有传入 `object` 的情况下**，`result` 会被初始化成空对象，否则调用 `initCloneObject` 来初始化。

如果不需要深度复制，则在需要复制原型链，也即 `isFlat` 为真值的情况下：
```javascript
copySymbolsIn(value, copyObject(value, keysIn(value), result))
```

可以看到，首先调用 `keysIn(value)` 将自身及原型链上所有非 `Symbol` 类型的可枚举属性收集，然后调用 `copyObject` 将非 `Symbol` 属性值复制到 `result` 中，再调用 `copySymbolsIn` 将自身及原型链上 `Symbol` 类型的可枚举属性值也复制到 `result` 中。

如果不需要复制原型链：

```javascript
copySymbols(value, Object.assign(result, value))
```

就简单很多了，使用 `Object.assign` 复制自身可枚举的非 `Symbol` 属性值到 `result` 中，再调用 `copySymbols` 将自身可枚举的 `Symbol` 属性值复制到 `result` 中即可。

从以上的分析中可以看到，如果单纯传入的 `value` 为 `function` 类型，即类似 `baseClone(function () {})` 时，得到的会是一个 `object` 类型，并不是函数。

### 不可复制数据类型的处理

相关源码如下：

```javascript
if (isArr) {
  ...
} else {
  ...
  if (tag == objectTag || tag == argsTag || (isFunc && !object)) {
    ...
  } else {
    if (isFunc || !cloneableTags[tag]) {
      return object ? value : {}
    }
    result = initCloneByTag(value, tag, isDeep)
  }
}
```

在上一节如果 `value` 为 `Function` 并且没有传入 `object` 时，得到的结果是对象。

在这个分支里，如果 `value` 为 `Function` 并且有传 `object` ，则得到的结果是 `value` 本身，即不会做任何复制。

我们在使用 `clone` 函数时，大部分情况下都不会直接传一个 `function` 去复制的，如果我们在复制一个包含函数的 `object` 时，得到的结果中，对应的函数还是指向原来的函数引用，即函数不会复制。

对于不可复制的数据类型的处理和函数的处理类似，如果直接传入一个不可复制的数据类型，会得到一个空对象，如果不可复制的数据类型包含是和 `object` 一起传入的，复制后，还是原来的值，即指向同一个引用。

如果不是以上的情况，则调用 `initCloneByTag` 函数初始化。

### 循环引用的解决

在深度复制的情况下，很容易出现循环引用的情况，`baseClone` 里使用 `stack` 来解决循环引用的问题。

相关代码如下：

```javascript
stack || (stack = new Stack)
const stacked = stack.get(value)
if (stacked) {
  return stacked
}
stack.set(value, result)
```

解决的思路其实很简单，以原始值 `value` 为 `key`，复制的结果 `result` 作为值，缓存到 `stack` 中。

在复制之前，先从 `stack` 中获取 `value` 的复制结果值，如果有值，表示之前已经对 `value` 做过复制，直接将结果返回即可。

如果没有值，则将 `result` 存入 `stack` 中。

### Map的复制

相关代码如下：

```javascript
if (tag == mapTag) {
  value.forEach((subValue, key) => {
    result.set(key, baseClone(subValue, bitmask, customizer, key, value, stack))
  })
  return result
}
```

调用 `value` 的 `forEach` 方法遍历，因为上面调用 `initCloneByTag` 的时候已经创建了一个 `Map` 的实例容器 `result` ，在遍历的过程中，调用 `baseClone` 将  `subValue` 复制，也即对 `value` 中每一个值都复制了一遍。

### Set的复制

相关代码如下：
```javascript
if (tag == setTag) {
  value.forEach((subValue) => {
    result.add(baseClone(subValue, bitmask, customizer, subValue, value, stack))
  })
  return result
}
```

`Set` 的复制和 `Map` 的复制类型，都是遍历，然后调用 `baseClone` 复制每个值，不再详述。

### TypedArray 的复制

相关代码如下：

```javascript
if (isTypedArray(value)) {
  return result
}
```

因为在 `initCloneByTag` 中已经调用了 `TypedArray` 相关的复制方法复制，也即 `result` 就是复制后的值，直接返回即可。

### 数组及对象的深度复制

相关源码如下：

```javascript
const keysFunc = isFull
? (isFlat ? getAllKeysIn : getAllKeys)
: (isFlat ? keysIn : keys)

const props = isArr ? undefined : keysFunc(value)
arrayEach(props || value, (subValue, key) => {
  if (props) {
    key = subValue
    subValue = value[key]
  }
  assignValue(result, key, baseClone(subValue, bitmask, customizer, key, value, stack))
})
```

#### 属性收集函数

在不同的情况下会不用不同的属性收集函数：

```javascript
const keysFunc = isFull
? (isFlat ? getAllKeysIn : getAllKeys)
: (isFlat ? keysIn : keys)

const props = isArr ? undefined : keysFunc(value)
```

`isFull` 表示是否需要收集 `Symbol` 类型的属性。

如果需要，则使用 `getAllKeysIn` 或者 `getAllKeys` 函数。

如果不需要，则使用 `keysIn` 或者 `keys` 函数。

`isFlat` 表示是否需要收集原型链上的属性。

如果需要收集原型链上的属性，则使用 `getAllKeysIn` 或者 `keysIn` 函数。

如果不需要，则使用 `getAllKeys` 或者 `keys`  函数。

这里就是根据 `isFull` 和 `isFlat` 不同的情况决定使用那一个函数来收集属性。

如果是数组，则不收集属性，如果不是，则收集属性到 `props` 中。

#### 遍历

```javascript
arrayEach(props || value, (subValue, key) => {
  if (props) {
    key = subValue
    subValue = value[key]
  }
  assignValue(result, key, baseClone(subValue, bitmask, customizer, key, value, stack))
})
```

使用 `arrayEach` 遍历属性集合 `props` 或者数组 `value` 。

如果 `value` 是数组，则每次遍历时， `subValue` 就是当前项的值，`key` 则为当前索引值。

如果 `props` 存在，则表示当前遍历的不是数组，而是对象，则 `subValue` 为当前的属性，使用 `subValue` 覆盖变量 `key` ，再从 `value` 中将 `key` 的值取出，覆盖变量 `subValue` ，这样就和数组遍历时的 `subValue` 和 `key` 含义对应起来。

接着递归调用 `baseClone` 对 `subValue` 进行复制，然后使用 `assignValue` 将复制后的值设置到结果 `result` 对应的 `key` 上，这样就达到了数组和对象复制的目的。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 