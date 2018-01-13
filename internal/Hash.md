# lodash源码分析之Hash缓存

> 在那小小的梦的暖阁，我为你收藏起整个季节的烟雨。
>
> ——洛夫《灵河》

本文为读 lodash 源码的第四篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 作用与用法

`Hash` 顾名思义，就是要有一个离散的序列，根据 `key` 来储取数据。而在 javascript 中，最适合的无疑是对象了。

`Hash` 在 lodash 的 `.internal` 文件夹中，作为内部文件来使用。lodash 会根据不同的数据类型选择不同的缓存方式，`Hash` 便是其中的一种方式，这种方式只能缓存 `key` 的类型符合对象键要求的数据。

 `Hash` 只接收一个二维数组作为参数，调用方式如下：

```javascript
new Hash([['test1', 1],['test2',2],['test3',3]])
```

其中子项中的第一项会作为 `key` ，第二项是需要缓存的值。

`Hash` 实例化的结果如下：

```javascript
{
  size: 3,
  __data__: {
    test1: 1,
    test2: 2,
    test3: 3
  }
}
```

缓存的数量储存在 `__data__` 的对象中。

## Hash与Map

后面将会讲到，除了使用 `Hash` 方式缓存数据外，还会用到 `Map`，lodash 在设计 `Hash` 的数据管理接口时，也与 `Map` 的接口一致，但是不会包含 `Map` 的遍历方法。

先来看看这些接口都有那些：

![](../images/hash.png)

## 源码

```javascript
const HASH_UNDEFINED = '__lodash_hash_undefined__'

class Hash {
  constructor(entries) {
    let index = -1
    const length = entries == null ? 0 : entries.length

    this.clear()
    while (++index < length) {
      const entry = entries[index]
      this.set(entry[0], entry[1])
    }
  }
  clear() {
    this.__data__ = Object.create(null)
    this.size = 0
  }
  delete(key) {
    const result = this.has(key) && delete this.__data__[key]
    this.size -= result ? 1 : 0
    return result
  }
  get(key) {
    const data = this.__data__
    const result = data[key]
    return result === HASH_UNDEFINED ? undefined : result
  }
  has(key) {
    const data = this.__data__
    return data[key] !== undefined
  }
  set(key, value) {
    const data = this.__data__
    this.size += this.has(key) ? 0 : 1
    data[key] = value === undefined ? HASH_UNDEFINED : value
    return this
  }
}

export default Hash
```

### constructor

```javascript
constructor(entries) {
    let index = -1
    const length = entries == null ? 0 : entries.length

    this.clear()
    while (++index < length) {
      const entry = entries[index]
      this.set(entry[0], entry[1])
    }
  }
```

在 `constructor` 中并没有看到初始化 `__data__` 属性和 `size` 属性，这个其实在 `clear` 方法中初始化了，后面会解释。

接着遍历传入的二维数组，调用 `set` 方法，初始化缓存的值。将子项的第一项作为 `key` ，第二项为缓存的值。

### clear

```javascript
clear() {
  this.__data__ = Object.create(null)
  this.size = 0
}
```

`clear` 的作用是清空缓存，因此需要将 `size` 重置为 `0`。

将缓存的数据 `__data__` 设置为空对象。

这里并没有用 `this.__data__ = {}` 置空，而是调用了 `Object.create` 方法，并且将 `null` 作为参数。我们都知道， `Object.create` 的第一个参数为创建对象的原型对象，传入 `null` 的时候，返回的就是一个真空对象，即没有原型的对象，因此不会有原型属性的干扰，用来做缓存对象十分适合。

### has

```javascript
has(key) {
  const data = this.__data__
  return data[key] !== undefined
}
```

`has` 用来判断是否已经有缓存数据，如果缓存数据已经存在，则返回 `true` 。

判断也十分简单，只需要判断取出来的值是否为 `undefined` 即可。

这个判断有一个坑，后面会讲到。

### set

```javascript
set(key, value) {
  const data = this.__data__
  this.size += this.has(key) ? 0 : 1
  data[key] = value === undefined ? HASH_UNDEFINED : value
  return this
}
```

`set` 用来增加或者更新需要缓存的值。`set` 的时候需要同时维护 `size` 和在缓存的值。

首先调用 `has` 方法，判断对应的 `key` 是否已经被缓存过，如果已经缓存过，则 `size` 保持不变，否则 `size` 加 `1` 。

缓存值其实就是设置缓存对象 `this.__data__` 对应 `key` 属性的值。

在 `has` 中说到用 `data[key] !== undefined` 有一个坑，因为要缓存的值也可以是 `undefined` ，如果不做处理，肯定会导致判断错误。

lodash 的处理方式是将 `undefined` 的值转换成 `HASH_UNDEFINED` ，也即一开始便定义的 `__lodash_hash_undefined__` 字符串来储存。

所以在缓存中，是用字符串 `__lodash_hash_undefined__` 来替代 `undefined` 的。

`set` 在最后还将实例 `this` 返回，以支持链式操作。

### get

```javascript
get(key) {
  const data = this.__data__
  const result = data[key]
  return result === HASH_UNDEFINED ? undefined : result
}
```

`get` 方法是从缓存中取值。

取值其实就是返回缓存对象中对应 `key` 的值即可。因为 `undefined` 在缓存中是以 `__lodash_hash_undefined__` 来表示的，因此遇到值为 `__lodash_hash_undefined__` 时，返回 `undefined` 。

其实这样还是有小小的问题的，如果需要缓存的值刚好是 `__lodash_hash_undefined__`，那取出来的值跟预设的就不一致了。但是这样情况应该很少出现吧。

如果是自己写的函数，可以用数组、对象或者 `Symbol` 来替代字符串，就不会出现字符串冲突的情况了。lodash 为什么不用呢，因为 lodash 是分模块发布的，不同的模块可能依赖不同版本的 `Hash` 类，这样 `HASH_UNDEFINED` 指向的内存或者 `Symbol` 值就不一致了，也就无法区分出 `undefined` 了。具体见作者的回复：[HASH_UNDEFINED why not use Object or Array](https://github.com/lodash/lodash/issues/3573)

### delete

```javascript
delete(key) {
  const result = this.has(key) && delete this.__data__[key]
  this.size -= result ? 1 : 0
  return result
}
```

`delete` 方法用来删除指定 `key` 的缓存。成功删除返回 `true`， 否则返回 `false`。 删除操作同样需要维护 `size` 属性和缓存值。

首先调用 `has` 方法来判断缓存是否存在，如果存在，用 `delete` 操作符将 `__data__` 中对应的属性删除。

`delete` 操作符在成功删除属性时会返回 `true`，如果成功删除，则需要将 `size` 减少 `1` 。

## 参考

1. [Set 和 Map 数据结构](http://es6.ruanyifeng.com/#docs/set-map#Map)
2. [Object.create()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/create)

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注，欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面

