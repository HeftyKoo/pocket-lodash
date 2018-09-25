# lodash源码分析之数据类型获取的兼容性

> 焦虑和恐惧的区别是，恐惧是对世界上的存在的恐惧，而焦虑是在"我"面前的焦虑。
>
> ——萨特《存在与虚无》

本文为读 lodash 源码的第十九篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 前言

在前文《[lodash源码分析之获取数据类型](baseGetTag.md)》已经解释了获取数据类型的方法，但是在有些环境下，一些 `es6` 新增的对象获取到的类型都为 `[object Object]` ，这样就没办法做细致的区分。例如在 IE11 中，通过 `Object.prototype.toString` 获取到的 `DataView` 对象类型为 `[object Object]`。 因此在 `getTag` 中，lodash 针对这些对象做了一些兼容性的事情。

## 依赖

```javascript
import baseGetTag from './baseGetTag.js'
```

《[lodash源码分析之获取数据类型](baseGetTag.md)》

## 源码分析

```javascript
const dataViewTag = '[object DataView]'
const mapTag = '[object Map]'
const objectTag = '[object Object]'
const promiseTag = '[object Promise]'
const setTag = '[object Set]'
const weakMapTag = '[object WeakMap]'

/** Used to detect maps, sets, and weakmaps. */
const dataViewCtorString = `${DataView}`
const mapCtorString = `${Map}`
const promiseCtorString = `${Promise}`
const setCtorString = `${Set}`
const weakMapCtorString = `${WeakMap}`

let getTag = baseGetTag

// Fallback for data views, maps, sets, and weak maps in IE 11 and promises in Node.js < 6.
if ((DataView && getTag(new DataView(new ArrayBuffer(1))) != dataViewTag) ||
    (getTag(new Map) != mapTag) ||
    (getTag(Promise.resolve()) != promiseTag) ||
    (getTag(new Set) != setTag) ||
    (getTag(new WeakMap) != weakMapTag)) {
  getTag = (value) => {
    const result = baseGetTag(value)
    const Ctor = result == objectTag ? value.constructor : undefined
    const ctorString = Ctor ? `${Ctor}` : ''

    if (ctorString) {
      switch (ctorString) {
        case dataViewCtorString: return dataViewTag
        case mapCtorString: return mapTag
        case promiseCtorString: return promiseTag
        case setCtorString: return setTag
        case weakMapCtorString: return weakMapTag
      }
    }
    return result
  }
}
```

`getTag` 的源码很简单，处理的是 `DataView`、`Map`、`Set`、`Promise`、`WeakMap` 等对象，下面就关键的几点说明一下。

### 函数的toString方法

```javascript
const dataViewCtorString = `${DataView}`
const mapCtorString = `${Map}`
const promiseCtorString = `${Promise}`
const setCtorString = `${Set}`
const weakMapCtorString = `${WeakMap}`
```

我们都知道，`DataView` 这些其实都是构造函数，函数有 `toString` 的方法，调用后返回的是 `function DataView() { [native code] }` 这样的格式，因为其实例调用 `Object.prototype.toString` 在某些环境下返回的是 `[object Object]`，而构造函数的 `toString` 返回的字符串中，包含了构造函数名，可以通过这点来区分。

### 实例中构造函数的获取

```javascript
 const Ctor = result == objectTag ? value.constructor : undefined
 const ctorString = Ctor ? `${Ctor}` : ''
```

每个实例中都包含一个 `constructor` 的属性，这个属性指向的是实例的构造函数，在获取到这个构造函数后，就可以调用它的 `toString` 方法，然后就可以比较了。

### Promise.resolve

```javascript
getTag(Promise.resolve()) != promiseTag
```

在条件判断时，使用了 `Promise.resolve()` ，这样使用的目的是获取到 `promise` 对象，因为 `Promise` 是一个构造函数，如果直接调用 `Object.prototype.toString`，返回的是 `[object Function]`。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面