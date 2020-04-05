# lodash源码分析之Stack

本文为读 lodash 源码的第一百八十一篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import ListCache from './ListCache.js'
import MapCache from './MapCache.js'
```

[《lodash源码分析之List缓存》](./ListCache.md)

[《lodash源码分析之缓存方式的选择》](./MapCache.md)

## 源码分析

`lodash` 中设计了 `Hash` 、`Map` 和 `List` 等数据结构来做缓存。

其中 `Hash` 本质上是使用对象来做缓存，这会使得 `Hash` 会在一些场景上会有使用限制，只能使用字符串或者 `Symbol` 来做 `key` 。但是 `Map` 和 `List` 则没有这些限制。

`Stack` 所做的，其实是动态切换 `Map` 和 `List` 缓存，因为 `List` 本质上是使用数组对来进行缓存，当缓存比较大时，会有性能问题，因此在缓存长度超过一定数量时，会切换到 `Map` 缓存。

`Stack` 所提供的接口和 `Map` 及 `List` 一致。

源码如下：

```javascript
const LARGE_ARRAY_SIZE = 200
class Stack {

  constructor(entries) {
    const data = this.__data__ = new ListCache(entries)
    this.size = data.size
  }

  
  clear() {
    this.__data__ = new ListCache
    this.size = 0
  }

  
  delete(key) {
    const data = this.__data__
    const result = data['delete'](key)

    this.size = data.size
    return result
  }

 
  get(key) {
    return this.__data__.get(key)
  }

  
  has(key) {
    return this.__data__.has(key)
  }

 
  set(key, value) {
    let data = this.__data__
    if (data instanceof ListCache) {
      const pairs = data.__data__
      if (pairs.length < LARGE_ARRAY_SIZE - 1) {
        pairs.push([key, value])
        this.size = ++data.size
        return this
      }
      data = this.__data__ = new MapCache(pairs)
    }
    data.set(key, value)
    this.size = data.size
    return this
  }
}
```

### constructor

```javascript
constructor(entries) {
  const data = this.__data__ = new ListCache(entries)
  this.size = data.size
}
```

传入的是`[[key, value], ...]` 这样形式的数组，初始化一个 `ListCache` 实例，用私有属性 `__data__` 保存 `ListCache` 实例，后面可能会切换成 `MapCache` 实例。

用 `size` 来保存缓存的长度。

### clear

```javascript
clear() {
  this.__data__ = new ListCache
  this.size = 0
}
```

`clear` 方法来清空缓存。

其实就是新创建一个 `ListCache` 实例，但是没有传入任何参数给 `constructor` 。

`size` 也重置为 `0` 。

### delete

```javascript
delete(key) {
  const data = this.__data__
  const result = data['delete'](key)

  this.size = data.size
  return result
}
```

其实就是调用 `ListCache` 或者 `MapCache` 上的 `delete` 方法，删除对应的 `key` 。

在删除的同时，也更新 `size` 。

### get

```javascript
get(key) {
  return this.__data__.get(key)
}
```

获取缓存上的值，也是简单的方法委托。

### has

```javascript
has(key) {
  return this.__data__.has(key)
}
```

判断某个 `key` 是否已有缓存，也是简单的方法委托。

### set

`Stack` 最主要的是 `set` 的方法，就是在这个方法里实现了 `List` 和 `Set` 数据结构的切换。

源码如下：

```javascript
set(key, value) {
  let data = this.__data__
  if (data instanceof ListCache) {
    const pairs = data.__data__
    if (pairs.length < LARGE_ARRAY_SIZE - 1) {
      pairs.push([key, value])
      this.size = ++data.size
      return this
    }
    data = this.__data__ = new MapCache(pairs)
  }
  data.set(key, value)
  this.size = data.size
  return this
}
```

首先判断 `data` 是否为 `ListCache` 的实例，如果是 `ListCache` 的实例，则从实例的私有属性 `__data__` 里取出原始数据 `pairs`，`pairs` 的形式如：`[[key, value],...]`

实现数据结构的切换关键在于 `pairs` 的长度，`lodash` 这里设定的长度为 `LARGE_ARRAY_SIZE - 1` ，即在小于 `199` 时，会使得 `ListCache` 。

如果使用的是 `ListCache` ，则将 `[key, value]` 直接 `push` 进 `pairs` 里即可，同时更新 `size` 和 `ListCache` 实例的 `size`。即 `this.size = ++data.size` 。

这里不直接使用 `ListCache` 的实例的 `set` 方法，我猜应该是出于性能的考虑，因为 `ListCache` 实例的 `set` 方法会做重复性检测，这会损耗一部分性能。而 `Stack` 是内部方法，在使用的时候会避免重复，因此在 `set` 的时候不需要这部分检测。

否则就切换成 `MapCache` 结构，构建实例时将 `pairs` 传给 `MapCache` 的构造方法。

接下来就是用 `MapCache` 实例的 `set` 方法来设置 `key` 和 `value` ，然后再更新 `size` 。

### 题外话

`Stack` 手动来管理 `size`，而且在多个地方都要更新 `size` ，其实可以利用 `getter` 来避免手动管理。

代码如下：
```javascript
class Stack {
  get size () {
    return this.__data__.size
  }
}
```

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 