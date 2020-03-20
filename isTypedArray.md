# lodash源码分析之isTypedArray

本文为读 lodash 源码的第一百二十八篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import getTag from './.internal/getTag.js'
import nodeTypes from './.internal/nodeTypes.js'
import isObjectLike from './isObjectLike.js'
```

[《lodash源码分析之数据类型获取的兼容性》](internal/getTag.md)
[《lodash源码分析之nodeTypes》](internal/nodeTypes.md)
[《lodash源码分析之isObjectLike》](./isObjectLike.md)

## 源码分析

`isTypedArray` 用来检测某个数据是否为 `typedArray` 类型。

在 `Node` 环境中，直接使用 `util` 的 `isTypedArray` 方法来检测。

在浏览器环境中，则使用 `Object.prototype.toString` ，然后通过正则来匹配。

源码如下：

```javascript
const reTypedTag = /^\[object (?:Float(?:32|64)|(?:Int|Uint)(?:8|16|32)|Uint8Clamped)Array\]$/

const nodeIsTypedArray = nodeTypes && nodeTypes.isTypedArray
const isTypedArray = nodeIsTypedArray
  ? (value) => nodeIsTypedArray(value)
  : (value) => isObjectLike(value) && reTypedTag.test(getTag(value))
```

### typedArray的类型

`typedArray` 有以下八种类型：

* **`Int8Array`**
* **`Uint8Array`**
* **`Uint8ClampedArray`**
* **`Int16Array`**
* **`Uint16Array`**
* **`Int32Array`**
* **`Uint32Array`**
* **`Float32Array`**
* **`Float64Array`**

`reTypedTag` 用来匹配`Object.prototype.toString` 后的结果是否符合这八种类型中的一种。

### 直接调用 `util` 的 `isTypedArray` 方法

如果 `nodeTypes.isTypedArray` 存在，则直接调用这个方法来判断。这个方法存在于 `Node.js` 的环境中。

```javascript
(value) => nodeIsTypedArray(value)
```

### 使用 `toString` 配合正则检测

如果没有 `nodeTypes.isTypedArray`  方法，则使用 `toString` 的方式配合正则来检测：

```javascript
(value) => isObjectLike(value) && reTypedTag.test(getTag(value))
```

先用 `isObjectLike` 来判断是否为对象类型，如果不是，则可以直接不用再检测。

## 参考资料

[ECMAScript 6 入门-ArrayBuffer](https://es6.ruanyifeng.com/#docs/arraybuffer)

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 