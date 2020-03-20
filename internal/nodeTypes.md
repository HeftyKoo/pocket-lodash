# lodash源码分析之nodeTypes

本文为读 lodash 源码的第一百二十七篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import freeGlobal from './freeGlobal.js'
```

[《lodash源码分析之freeGlobal》](./freeGlobal.md)

## 源码分析

`nodeTypes` 是 `node` 的 `util` 中 `types` 的方法集合，主要用来检测数据类型。

### CommonJS 模块加载机制检测

这个检测的源码和上一篇文章[《lodash源码分析之isBuffer》](../isBuffer.md)中已有详细的分析，源码如下：

```javascript
const freeExports = typeof exports === 'object' && exports !== null && !exports.nodeType && exports
const freeModule = freeExports && typeof module === 'object' && module !== null && !module.nodeType && module
const moduleExports = freeModule && freeModule.exports === freeExports
```

### 直接从utils上取出

根据 `Node.js` `util` 模块的使用方式，可以直接用 `require` 方法，将 `util` 模块加载进来。

源码如下：

```javascript
const typesHelper = freeModule && freeModule.require && freeModule.require('util').types
```

### 使用 `process.binding`

在 `Node.js` `v10+` 版本之前，`util` 模块上是没有暴露出 `types` 的方法集的，在 `v10+` 之前，可以使用 `process.binding` 来加载 `util` 上 `types` 方法。

```javascript
const freeProcess = moduleExports && freeGlobal.process
freeProcess && freeProcess.binding && freeProcess.binding('util')
```

这个方法在文档上没有体现，但是可以在 [`Deprecated APIs`](https://nodejs.org/api/deprecations.html#deprecations_dep0103_process_binding_util_is_typechecks) 看到这个方法的使用方式。

最后整合出来的代码如下：

```javascript
const nodeTypes = ((() => {
  try {
    const typesHelper = freeModule && freeModule.require && freeModule.require('util').types
    return typesHelper
      ? typesHelper
      : freeProcess && freeProcess.binding && freeProcess.binding('util')
  } catch (e) {}
})())
```

为什么要用 `try...catch` 呢，因为在其他环境中，也可能有 `require` 或者 `process.binding` 方法，这些方法可能会导致出错，所以需要 `catch` 一下错误。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 