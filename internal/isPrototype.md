# lodash源码分析之isPrototype

本文为读 lodash 源码的第一百九十五篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`isPrototype` 用来判断传入的值是否为一个原型对象。

### new 创建实例的过程

在使用构造函数创建实例时，《Javascript 权威指南》有以下描述：

> 构造函数会自动创建对象，然后将构造函数作为这个对象的方法调用一次，最后返回这个新对象。

因此假设有以下的构造函数：

```javascript
function Test () {
  this.a = a
  this.b = b
}

Test.prototype.noop = function () {}
```

在使用 `new Test` 创建实例时，可以使用 `fakeNew` 函数大致还原其过程：

```javascript
function fakeNew () {
  var obj = {}
  Test.apply(obj, arguments)
  obj.__proto__ = Test.prototype
  return obj
}
```

这里的原型对象是 `obj.__proto__`，也即 `Test.prototype`。

任何函数都有一个 `prototype` 对象，这个对象里有一个属性 `constructor` ，`constructor` 属性的值指向的是原函数，在这里即是 `Test` 函数。

因此要判断一个对象是否为原型对象就比较简单了，只要判断这个对象是否有 `constructor` 属性，如果有 `constructor` 属性，则 `constructor` 的属性值应该为一个函数，并且这个函数的 `prototype` 属性就是这个对象。

### 边界条件处理

来看具体的源码：

```javascript
const objectProto = Object.prototype
function isPrototype(value) {
  const Ctor = value && value.constructor
  const proto = (typeof Ctor === 'function' && Ctor.prototype) || objectProto

  return value === proto
}
```

根据之前的知道，很容易就写出了以下的代码：

```javascript
function isPrototype(value) {
  const Ctor = value && value.constructor
  const proto = typeof Ctor === 'function' && Ctor.prototype

  return value === proto
}
```

但是，我们看到原代码中如果 `Ctro` 找不到`protype` 时，`proto` 还会默认成 `Object.protype`。

为什么要这样做呢？

假设传入的值为 `false` ，现在的 `isPrototype` 函数得到的 `proto` 也为 `false`，最终的结果会返回 `true` ，这明显不符合预期，因此这里用 `Object.prototype` 来做守卫。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 