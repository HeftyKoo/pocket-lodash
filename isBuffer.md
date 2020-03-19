# lodash源码分析之isBuffer

本文为读 lodash 源码的第一百二十六篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import root from './.internal/root.js'
```

[《lodash源码分析之root》](internal/root.md)

## 源码分析

`javascript` 原本没有二进制数据类型 `Buffer` ，但是在处理 `TCP` 流或者文件流时，又必须要用到 `Buffer` ，因此 `Node.js` 定义了一个 `Buffer` 的数据类型。

`Node.js` 定义了一个 `Buffer` 类，`Buffer` 类上有一个静态方法 `isBuffer` ，来检测某个值是否为 `Buffer` 类型。

因为 `lodash` 是跨平台的，因此第一步是猜测是否在 `Node.js` 环境中，如果很可能是 `Node.js` 环境中，则从顶层对象中取 `Buffer` 类中的 `isBuffer` 方法。

 ### CommonJS 检测

检测是否在 `Node.js` 环境中，`lodash` 选择检测当前环境中是否支持 `CommonJS` 模块加载机制。

#### exports 对象检测

`Node.js` 原生支持 `CommonJS` 模块加载机制，在全局环境上会暴露 `module` 对象和 `exports` 对象。

检测 `exports` 对象的源码如下：

```javascript
const freeExports = typeof exports === 'object' && exports !== null && !exports.nodeType && exports
```

`typeof exports === 'object' && exports !== null` 这步是确定 `exports` 是 `object` 类型并且不为 `null` 。

`!exports.nodeType` 这步是排除 `exports` 对象是 `html` 元素。

在什么情况下 `exports` 会为 `html` 元素呢，最常见的是，将某个元素的 `id` 设置为 `exports` ，浏览器就会自动自动增加一个 `exports` 变量，指向改元素。

#### module对象检测

源码如下：

```javascript
const freeModule = freeExports && typeof module === 'object' && module !== null && !module.nodeType && module
```

原理和 `exports` 一样，不过更严格，首先判断是否存在 `exports` 对象。因为这两个在 `Node.js` 中肯定是同时存在的。

#### exports 为 module.exports 的引用检测

`CommonJS` 规定，`exports` 对象必须为 `module.exports` 的引用。

源码如下：

```javascript
const moduleExports = freeModule && freeModule.exports === freeExports
```

这样就检测了当前环境是否支持 `CommonJS` 模块加载机制。

### isBuffer 方法

虽然已经检测当前环境支持 `CommonJS` 模块加载机制，但是在浏览器上要满足这些条件也很简单，因此还不能直接确定当前环境是否有 `Buffer` 对象和 `isBuffer` 方法。

因此，还需要按以下的方式取值：

```javascript
const Buffer = moduleExports ? root.Buffer : undefined
const nativeIsBuffer = Buffer ? Buffer.isBuffer : undefined
```

每次取值时都做一次空值检测。

最后，就是 `lodash` 所暴露的 `isBuffer` 方法：

```javascript
const isBuffer = nativeIsBuffer || (() => false)
```

其实就是将原生的 `isBuffer` 方法暴露出去，如果 `isBuffer` 方法不存在，则暴露出去的是一个永远返回 `false` 值的方法。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 