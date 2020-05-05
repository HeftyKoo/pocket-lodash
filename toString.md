# lodash源码分析之toString

本文为读 lodash 源码的第二百五十一篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import isSymbol from './isSymbol.js'
```

[《lodash源码分析之isSymbol》](isSymbol.md)

## 源码分析

`toString` 的作用是将 `value` 转换成 `string` 类型。

源码如下：

```javascript
const INFINITY = 1 / 0
function toString(value) {
  if (value == null) {
    return ''
  }
  if (typeof value === 'string') {
    return value
  }
  if (Array.isArray(value)) {
    return `${value.map((other) => other == null ? other : toString(other))}`
  }
  if (isSymbol(value)) {
    return value.toString()
  }
  const result = `${value}`
  return (result == '0' && (1 / value) == -INFINITY) ? '-0' : result
}

```

### 处理空值

```javascript
if (value == null) {
  return ''
}
```

如果传入的是 `undefined` 或者 `null`，返回空字符串。

### 处理字符串

```javascript
if (typeof value === 'string') {
  return value
}
```

如果传入的就是字符串，原值返回即可。

### 处理数组

```javascript
if (Array.isArray(value)) {
  return `${value.map((other) => other == null ? other : toString(other))}`
}
```

如果 `value` 是数组，则使用数组的 `map` 方法遍历 `value` 中每一项，递归调用 `toString` ，将每一项都转换成字符串，得到一个字符串数组。

因为使用了模板字符串，所以这个字符串数组最终也会隐式转换成字符串。

### 处理Symbol类型

```javascript
if (isSymbol(value)) {
  return value.toString()
}
```

如果是 `Symbol` 类型，则调用 `value` 的 `toString` 方法，得到字符串。

### 处理其他值

```javascript
const result = `${value}`
return (result == '0' && (1 / value) == -INFINITY) ? '-0' : result
```

如果是其他数值类型，则使用模板字符串转换成字符串。

#### 处理-0

如果得到是字符串是 `0` ，有可能传入的 `value` 为 `-0` ，因此还要进一步判断 `1/value` 是否等于 `-INFINITY` ，如果是，则可以确定 `value` 为 `-0` ，返回字符串 `'-0'`。

为什么不处理 `+0` 呢？因为没办法区分 `+0` 和 `0` ，或者说 `+0` 也即是 `0` ，所以不做处理，得到的字符串都是 `'0'` 。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

