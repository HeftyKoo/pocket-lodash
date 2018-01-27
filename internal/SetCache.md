# lodash源码分析之缓存使用方式的进一步封装

> 在世界上所有的民族之中，支配着他们的喜怒选择的并不是天性，而是他们的观点。
>
> ——卢梭《社会与契约论》

本文为读 lodash 源码的第九篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 前言

在之前的《[lodash源码分析之Hash缓存](Hash.md)》和《[lodash源码分析之List缓存](ListCache.md)》介绍过 lodash 的两种缓存方式，在《[lodash源码分析之缓存方式的选择](MapCache.md)》中介绍过这两种缓存方式和 `Map` 的封装，lodash 会根据缓存类型来选择最优的缓存方式。

但是在 `MapCache` 类中，要初始化缓存和设置缓存都需要提供 `key` 和 `value` 组成的二维数组，因此在 `SetCache` 类中，lodash 提供了一种更方便的缓存设置方式，只需要提供缓存的值即可。

## 依赖

```javascript
import MapCache from './MapCache.js'
```

[lodash源码分析之缓存方式的选择](MapCache.md)

## 源码分析

```javascript
const HASH_UNDEFINED = '__lodash_hash_undefined__'

class SetCache {

  constructor(values) {
    let index = -1
    const length = values == null ? 0 : values.length

    this.__data__ = new MapCache
    while (++index < length) {
      this.add(values[index])
    }
  }

  add(value) {
    this.__data__.set(value, HASH_UNDEFINED)
    return this
  }

  has(value) {
    return this.__data__.has(value)
  }
}

SetCache.prototype.push = SetCache.prototype.add
```

### 总体思路

从源码中可以看到，`SetCache` 其实调用的是 `MapCache` 类，使用缓存的值作为 `key` ，所有的 `key` 对应的值都是 lodash 定义的标准 `undefined` 值 `HASH_UNDEFINED` ，正如之前文章中论述过的，这个值用于 `Hash` 缓存时，避免判断是缓存是否存在时出错。

判断缓存是否存在，只需要判断 `MapCache` 是否存在对应的 `key` 。

### constructor

```javascript
constructor(values) {
  let index = -1
  const length = values == null ? 0 : values.length

  this.__data__ = new MapCache
  while (++index < length) {
    this.add(values[index])
  }
}
```

这里构造函数不需要再传入 `key-value` 的二维数组了，只需要传入包含所有缓存值的数组即可。

`__data__` 属性保存的其实是 `MapCache` 的实例。

初始化时只需要遍历需要缓存的数组 `values` ，然后调用 `add` 方法，设置缓存即可。

### add

```javascript
add(value) {
  this.__data__.set(value, HASH_UNDEFINED)
  return this
}
```

`add` 方法用来设置缓存。

其实调用的是 `MapCahce` 实例的 `set` 方法，使用缓存值 `value` 作为 `key` ，用 `HASH_UNDEFINED` 作为缓存值。

 ### has

```javascript
has(value) {
  return this.__data__.has(value)
}
```

`has` 方法用于判断缓存是否存在。

只需要调用 `MapCache` 实例的 `has` 方法即可。

### push

```
SetCache.prototype.push = SetCache.prototype.add
```

`push` 方法只是 `add` 方法的别名。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面