# lodash源码分析之toSafeInteger

本文为读 lodash 源码的第二百五十篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import toInteger from './toInteger.js'
```

[《lodash源码分析之toInteger》](toInteger.md)

## 源码分析

`toSafeInteger` 是将 `value` 转换成安全整数。

`javascript` 使用64位双精度浮点数来储存数字类型，因此，它能表达安全储存的范围是 `-(253 - 1)` 到 `253 - 1` 之间的数值（包含边界值。

源码如下：

```javascript
const MAX_SAFE_INTEGER = 9007199254740991
function toSafeInteger(value) {
  if (!value) {
    return value === 0 ? value : 0
  }
  value = toInteger(value)
  if (value < -MAX_SAFE_INTEGER) {
    return -MAX_SAFE_INTEGER
  }
  if (value > MAX_SAFE_INTEGER) {
    return MAX_SAFE_INTEGER
  }
  return value
}
```

### 正负0处理

```java
if (!value) {
  return value === 0 ? value : 0
}
```

看到第一段你可能会有点奇怪，在假值的情况下，`value === 0 ` 返回的是 `value` ，其它的返回 `0` ，这不有点重复吗？为不会不是像以下这样就好了呢？

```javascript
if (!value) {
  return 0
}
```

这是因为 `0 `  还有 `+0` 和 `-0` 之分，并且 `+0` 或者 `-0` 在和 `0` 比较的时候，使用 `===` 时，返回的也是 `true` 。因此，在 `value === 0` 时，直接返回 `value` ，以保持原来的 `+0` 或者 `-0` 。

### 边界值处理

```javascript
value = toInteger(value)
if (value < -MAX_SAFE_INTEGER) {
  return -MAX_SAFE_INTEGER
}
if (value > MAX_SAFE_INTEGER) {
  return MAX_SAFE_INTEGER
}
return value
```

调用 `toInteger` 方法，将 `value` 转换成整数。

如果 `value` 小于最小安全整数，则返回最小安全整数，如果大于最大安全整数，则返回最大安全整数。

其它情况返回转换后的整数 `value` 。

源码如下：

```javascript
function toPlainObject(value) {
  value = Object(value)
  const result = {}
  for (const key in value) {
    result[key] = value[key]
  }
  return result
}
```

可以看到，先使用对象字面量创建一个空对象 `result` 。

然后使用 `for...in` 将 `value` 上可枚举（包括原型链但除了 `Symbol` 类型）属性遍历，在 `result` 上设置值。

这有点像是将 `value` 复制给了 `result` ，从而得到一个纯对象，这个纯对象的构造函数是 `Object` 。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 