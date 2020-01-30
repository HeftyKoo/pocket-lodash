# lodash源码分析之isKey

本文为读 lodash 源码的第六十五篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import isSymbol from '../isSymbol.js'
```

[《lodash源码分析之isSymbol》](../isSymbol.md)

## 源码分析

`isKey` 的作用是用来判断传入的 `value` 是否为合法的属性名（注意：不是规范中认定的合法属性名，`lodash` 会做一些收敛），但是在检测的时候会将 `lodash` 支持的嵌套属性的写法排除，如传入 `a.b.c` 或者 `a[0].b.c` 可能会返回 `false`。

源码如下：

```javascript
const reIsDeepProp = /\.|\[(?:[^[\]]*|(["'])(?:(?!\1)[^\\]|\\.)*?\1)\]/
const reIsPlainProp = /^\w*$/
function isKey(value, object) {
  if (Array.isArray(value)) {
    return false
  }
  const type = typeof value
  if (type === 'number' || type === 'boolean' || value == null || isSymbol(value)) {
    return true
  }
  return reIsPlainProp.test(value) || !reIsDeepProp.test(value) ||
    (object != null && value in Object(object))
}
```

### 排除数组

```javascript
if (Array.isArray(value)) {
  return false
}
```

如果传入的是数组，在 `lodash` 中，不会当作是属性名，直接排除，返回 `false`。但其实我们是可以用数组作为 `key` 的，用数组作为 `key` 时，数组会首先会转换成 `string` 类型，再作为对象的属性。 在 `lodash` 的 `isKey` 函数中，不允许这种行为。

### 常规属性名

```javascript
const type = typeof value
if (type === 'number' || type === 'boolean' || value == null || isSymbol(value)) {
  return true
}
```

接下来，检测 `value` 是否为数字、布尔值、`null` 值和是否为 `symbol` 类型，上述类型，除了 `symbol` 外，作为属性值时，都会隐式转换成 `string` 类型，也可以作为合法的属性名。而 `symbol` 在 `es6` 中，本身就允许作为对象的属性，不需要转换成 `string` 类型。

### 处理字符串类型

```javascript
return reIsPlainProp.test(value) || !reIsDeepProp.test(value) ||
    (object != null && value in Object(object))
```

现在还剩下字符串和对象没有处理，但是以上的条件中，对象也会隐式转换成字符串来处理，所以这里看字符串的处理即可。

#### 处理单词

```javascript
const reIsPlainProp = /^\w*$/
reIsPlainProp.test(value)
```

`\w` 是匹配a-z、A-Z、0-9，以及下划线的单词字符，`resIsPlainProp` 是用来匹配包含一个或多个这些字符的单词，也即类似于 `a1`、`ab` 、`a-b` 这类的字符串，如果是这些字符串，则返回 `true`。

#### 处理属性路径

```javascript
const reIsDeepProp = /\.|\[(?:[^[\]]*|(["'])(?:(?!\1)[^\\]|\\.)*?\1)\]/
!reIsDeepProp.test(value)
```

在 `lodash` 中，像 `a.b.c` 和 `a[0].b.c` 这种可能会被当作属性路径对待，像 `get` 这些的函数支持传入这样的路径来获取嵌套对象的值。

`reIsDeepProp` 这个正则比较复杂，主要就是用来匹配这种深层属性路径的，在 `isKey` 检测时，会将这些字符串排除。

除此之外的字符串，返回的都是 `true` 。

本来有这个条件就可以，为什么在这个条件之前还要有 `reIsPlainProp.test(value)` 这个条件呢？

因为这个正则比较复杂，性能会比较差，所以先匹配 `reIsPlainProp` 在大部分场景下性能会更加好。

#### 处理属性路径的特例

如果在某些场景中，真的要用属性路径来作为属性名怎么办呢？

这种情况下，使用 `isKey` 函数时，就需要传入第二个参数  `object` 了。

```javascript
object != null && value in Object(object)
```

如果有传 `object`，并且 `value` 是 `object` 的属性名，也会返回 `true` 。这就解决了属性路径作为属性名的问题。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 