# lodash源码分析之cloneBuffer

本文为读 lodash 源码的第一百八十二篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import root from './root.js'
```

[《lodash源码分析之root》](./root.md)

## 源码分析

`cloneDeep` 用来克隆 `Buffer` 数据类型。

源码如下：

```javascript
const freeExports = typeof exports === 'object' && exports !== null && !exports.nodeType && exports

const freeModule = freeExports && typeof module === 'object' && module !== null && !module.nodeType && module

const moduleExports = freeModule && freeModule.exports === freeExports

const Buffer = moduleExports ? root.Buffer : undefined, allocUnsafe = Buffer ? Buffer.allocUnsafe : undefined

function cloneBuffer(buffer, isDeep) {
  if (isDeep) {
    return buffer.slice()
  }
  const length = buffer.length
  const result = allocUnsafe ? allocUnsafe(length) : new buffer.constructor(length)

  buffer.copy(result)
  return result
}
```

`cloneBuffer` 函数之前的代码主要是做 `Node.js` 环境检测，并且将 `Buffer.allocUnsafe` 方法取出。

这部分逻辑跟 `isBuffer` 方法中的逻辑十分相似，在[《lodash源码分析之isBuffer》](../isBuffer.md)中已经做过分析，这里不再细说。

### 深度拷贝

```javascript
if (isDeep) {
  return buffer.slice()
}
```

如果 `isDeep` 为 `true` ，则使用 `buffer` 实例上的 `slice` 方法返回一个新的 `buffer` 。

但是根据文档的描述，这个新的 `buffer` 其实和原 `buffer` 指向的是同一块内存，如果原 `buffer` 有更改，新的 `buffer` 也会更改，同理，新的 `buffer` 的更改同样会影响到原 `buffer`。

### 浅拷贝

```javascript
const length = buffer.length
const result = allocUnsafe ? allocUnsafe(length) : new buffer.constructor(length)

buffer.copy(result)
return result
```

现在推荐使用 `Buffer.from`、`Buffer.alloc` 或者 `Buffer.allocUnsafe` 来开辟一块内存来创建 `buffer` ，不推荐直接使用 `new Buffer(length)` 的方式来创建。

`lodash` 会优先使用 `allocUnsafe` 来创建，因为这个方法在开辟内存的时候不会做初始化的操作，但是 `lodash` 在开辟完内存后并没有马上给外界使用这块内存，所以使用这个方法是安全的。因为没有初始化的操作，因此性能会更加高。

如果没有 `allocUnsafe` 方法，则使用 `new buffer.constructor` 来开辟一块长度为 `length` 的内存。这其实相当于 `new Buffer(length)` ，因为 `buffer` 实例的构造函数就是 `Buffer` ，但是使用 `new buffer.constructor` 会更加安全，因为用户有可能是通过继承 `Buffer` 类的形式来创建 `buffer` 的，甚至还有可能将全局的 `Buffer` 类覆盖了。

例如：

```javascript
const Buffer = Object
```

这样全局的 `Buffer` 类就被改写成 `Object` 类了。

最后使用 `buffer.copy` 方法将 `buffer` 复制到 `result` 中。

### 疑惑

为什么 `isDeep` 的时候使用 `slice` 让我一直疑惑。

因为根据文档，`slice` 会：

> Returns a new Buffer that references the same memory as the original

也即 `slice` 所返回的新 `Buffer` 和原 `Buffer` 指向的是同一块内存。

在和 `TypedArray.slice` 的对比中更有如下描述：

> While TypedArray#slice() creates a copy of part of the TypedArray, Buffer#slice() creates a view over the existing Buffer without copying.

清楚说明了 `slice` 是 `without copying` 的。

照理不能用 `slice` 来做深度拷贝，总之令人费解。

## 参考资料

[Node.js-Buffer](https://nodejs.org/docs/latest-v13.x/api/buffer.html)

[内功修炼之lodash—— clone& cloneDeep(一定有你遗漏的js基础知识) ](https://github.com/lhyt/issue/issues/53)

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 