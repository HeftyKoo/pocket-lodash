# lodash源码分析之root

本文为读 lodash 源码的第一百二十五篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import freeGlobal from './freeGlobal.js'
```

[《lodash源码分析之freeGlobal》](./freeGlobal.md)

## 源码分析

在[《lodash源码分析之freeGlobal》](./freeGlobal.md)介绍了 `global` 全局对象。

其实在 `javascript` 中还有不同的顶层全局对象。

### self

`self` 在浏览器中大部分情况下指向的是当前 `window` 引用，而在 `worker` 中，只有 `self` 这个顶层全局对象，是没有 `window` 对象的。

`self` 可以用来判断当前 `iframe` 在父级中的位置，例如 `MDN` 中有以下的示例代码：

```javascript
if (window.parent.frames[0] != window.self) {
    // 当前对象不是frames列表中的第一个时
 }
```

> **window.self** 几乎总是用于上面示例那样的比较，用来判断当前 window 是不是父 frameset 中的第一个 frame。

获取 `self` 的方式和 `global` 类似：

```javascript
const freeSelf = typeof self === 'object' && self !== null && self.Object === Object && self
```

### globalThis

既然在 `node` 中有 `global` 顶层对象，在浏览器环境下有 `window` 顶层对象，在 `worker` 中也有 `self` 顶层对象，那为什么还需要 `globalThis` 呢？

答案很简单，为了统一。

`MDN` 中有以下解释：

> `globalThis` 提供了一个标准的方式来获取不同环境下的全局 `this` 对象（也就是全局对象自身）

这样，使用 `globalThis` 可以确保代码在不同的环境下，都可以正常工作。

同理，获取 `globalThis` 的源码如下：

```javascript
const freeGlobalThis = typeof globalThis === 'object' && globalThis !== null && globalThis.Object == Object && globalThis
```

### this

在松散模式下，可以在函数中返回 `this` 获取全局对象，但是在严格模式下，`this` 会返回 `undefined`。

因此也可以使用以下代码来获取顶层全局对象：

```javascript
Function('return this')()
```

### 源码

考虑到这些情况之后，获取顶层代码的源码如下：

```javascript
const root = freeGlobalThis || freeGlobal || freeSelf || Function('return this')()
```

为什么没有 `window` 的捕获呢？因为在有 `window` 的环境中，`self` 肯定是 `window` 对象的引用。

这个顺序也很有意思，首先是 `globalThis`，因为这有最大的普适性，接着是 `global` ，因为在 `node` 的环境中，性能的考量会比浏览器环境更重要。

## 参考资料

[Node.js](https://nodejs.org/api/globals.html#globals_global)

[MDN: window.self](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/self)

[MDN: globalThis](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/globalThis)

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 