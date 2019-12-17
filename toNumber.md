# lodash源码分析之toNumber

本文为读 lodash 源码的第五十二篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import isObject from './isObject.js'
import isSymbol from './isSymbol.js'
```

[《lodash源码分析之isObject》](isObject.md)

[《lodash源码分析之isSymbol》](isSymbol.md)

## 源码分析

### 整体源码

```javascript
const NAN = 0 / 0

/** Used to match leading and trailing whitespace. */
const reTrim = /^\s+|\s+$/g

/** Used to detect bad signed hexadecimal string values. */
const reIsBadHex = /^[-+]0x[0-9a-f]+$/i

/** Used to detect binary string values. */
const reIsBinary = /^0b[01]+$/i

/** Used to detect octal string values. */
const reIsOctal = /^0o[0-7]+$/i

/** Built-in method references without a dependency on `root`. */
const freeParseInt = parseInt

function toNumber(value) {
  if (typeof value === 'number') {
    return value
  }
  if (isSymbol(value)) {
    return NAN
  }
  if (isObject(value)) {
    const other = typeof value.valueOf === 'function' ? value.valueOf() : value
    value = isObject(other) ? `${other}` : other
  }
  if (typeof value !== 'string') {
    return value === 0 ? value : +value
  }
  value = value.replace(reTrim, '')
  const isBinary = reIsBinary.test(value)
  return (isBinary || reIsOctal.test(value))
    ? freeParseInt(value.slice(2), isBinary ? 2 : 8)
    : (reIsBadHex.test(value) ? NAN : +value)
}

```

### 几个常量

```javascript
const NAN = 0 / 0

/** Used to match leading and trailing whitespace. */
const reTrim = /^\s+|\s+$/g

/** Used to detect bad signed hexadecimal string values. */
const reIsBadHex = /^[-+]0x[0-9a-f]+$/i

/** Used to detect binary string values. */
const reIsBinary = /^0b[01]+$/i

/** Used to detect octal string values. */
const reIsOctal = /^0o[0-7]+$/i

/** Built-in method references without a dependency on `root`. */
const freeParseInt = parseInt
```

#### NAN

`NaN` 的意思是 `not a number` ，用 `0 / 0` 可以返回一个 `NaN` 值。

#### reTrim

这个正则用来去除前后的空格

#### reIsBadHex

这个正则咋看起来有点奇怪，是检测形似带符号的16进制字符串，如： `+0x1f` 。为什么是 `bad` 呢，从 `lodash` 的[pr](https://github.com/lodash/lodash/pull/1577/commits/1c6de59e3112996ee93c0d8fd1b447f569f8bd21#diff-001d0647fb00f8336795faccdec19a31)来看，应该是跟 `node v0.8` 的某个 bug 有关。因为 `lodash` 也支持在 `node` ，因此需要做兼容。所以十六进制的处理跟二进制和八进制不太一样。

#### resIsBinary

形似二进制的字符串正则，二进制以 `0b` 开头，后面只能是 `0` 或者 `1` 。

#### reIsOctal

形似八进制的字符串正则，八进制以 `0o` 开头，后面只能跟 `0-7` 的数字。

### 处理number类型

```javascript
if (typeof value === 'number') {
  return value
}
```

如果为 `number` 类型，则不需要做转换，原样返回即可。

### 处理Symbol类型

```javascript
if (isSymbol(value)) {
  return NAN
}
```

如果为 `Symbol` 类型，则直接返回 `NAN` 。

### 处理Object类型

```javascript
if (isObject(value)) {
  const other = typeof value.valueOf === 'function' ? value.valueOf() : value
  value = isObject(other) ? `${other}` : other
}
```

规范中有规定，`object` 类型有 `valueOf` 的方法，这个方法可以返回对象的原始值，默认情况下，如果 `valueOf` 没有返回原始值，则会返回对象本身。

例如 `Number(1)` 的 `valueOf` 会返回数字 `1` 。以下的例子会返回 `a` ：

```javascript
const obj = {
  a: 'a',
  valueOf () {
    return 'a'
  }
}
```

因此，首先判断 `value` 有没有存在 `valueOf` 函数，如果有，则先调用 `valueOf` 得出结果。

但是，`value` 可能会没有 `valueOf` 函数，或者 `valueOf` 函数也可能会返回一个对象，这时，需要将 `other` 转换成 `string` 类型，留待后面处理。

### 处理非String类型

```javascript
if (typeof value !== 'string') {
  return value === 0 ? value : +value
}
```

在处理完 `Object` 类型后，现在 `value` 都是基本类型了，在这个阶段，本来可以直接用 `+value` 来转换成 `number` 类型的，但是字符串前后可能会有空格，直接用 `+value` 来转换的话，会得出 `NaN` 值，因此字符串需要单独处理。

### 处理String类型

```javascript
value = value.replace(reTrim, '')
const isBinary = reIsBinary.test(value)
return (isBinary || reIsOctal.test(value))
  ? freeParseInt(value.slice(2), isBinary ? 2 : 8)
: (reIsBadHex.test(value) ? NAN : +value)
```

第一步先将 `value` 的前后空格移除。

接下来处理二进制和八进制，直接用 `parseInt` 来做转换，`parseInt` 的第二个参数来指定进制，其实二进制、八进制也可以用 `+value` 来转换的，有也人提了 [pr](https://github.com/lodash/lodash/pull/4230)，而且性能更好。

本来 `16` 进制也可以用 `parseInt` 来处理的，但是根据 `lodash` 的提交来推测，用 `parseInt` 来转换形似  `+0x16` 或 `-0x16` 的字符串时，会有bug，这样的字符串，预期应该返回 `NAN` 。

如果不是形似二进制、八进制或带符号的十六进制字符串，则使用 `+value` 来做转换。

## 参考资料

[MDN: Object.prototype.valueOf()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/valueOf)

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 