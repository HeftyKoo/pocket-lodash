# lodash源码分析之createPadding

本文为读 lodash 源码的第三百二十六篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import repeat from '../repeat.js'
import baseToString from './baseToString.js'
import castSlice from './castSlice.js'
import hasUnicode from './hasUnicode.js'
import stringSize from './stringSize.js'
import stringToArray from './stringToArray.js'
```

[lodash源码分析之repeat](../repeat.md)

[lodash源码分析之baseToString](./baseToString.md)

[lodash源码分析之castSlice](./castSlice.md)

[lodash源码分析之hasUnicode](./hasUnicode.md)

[lodash源码分析之stringSize](./stringSize.md)

[lodash源码分析之stringToArray](./stringToArray.md)

## 源码分析

`createPadding` 其实也可以理解成 `repeat` 和 `slice` 的组合，它接收一个字符串 `chars` 和长度 `length` ，最终会返回一个长度为 `length` 的字符串。

如果 `chars` 的长度不足 `length` ， 会重复 `chars` ，得到一个长度为 `length` 的字符串，如果 `chars` 的长度大于 `length` ，则会对 `chars` 进行截断。

源码如下：

```javascript
function createPadding(length, chars) {
  chars = chars === undefined ? ' ' : baseToString(chars)

  const charsLength = chars.length
  if (charsLength < 2) {
    return charsLength ? repeat(chars, length) : chars
  }
  const result = repeat(chars, Math.ceil(length / stringSize(chars)))
  return hasUnicode(chars)
    ? castSlice(stringToArray(result), 0, length).join('')
    : result.slice(0, length)
}
```

### 对chars进行转换

```javascript
chars = chars === undefined ? ' ' : baseToString(chars)
```

如果 `chars` 为 `undefined` ，则默认 `chars` 为一个空格。

否则调用 `baseToString` 将 `chars` 转换成字符串。

### 处理chars长度小于2的情况

```javascript
const charsLength = chars.length
if (charsLength < 2) {
  return charsLength ? repeat(chars, length) : chars
}
```

`charsLength` 小于 `2`，也即为 `0` 或者 `1` 的时候，不涉及到截断操作。

当 `charsLength` 有值时，也即为 `1` 时，调用 `repeat` 将 `chars` 重复 `length` 次即可，即使 `length` 为 `0` ，`repeat` 得到是将会是一个空字符串，不需要再进行额外的截断操作。

当 `charsLength` 为 `0` 时，即本身就是一个空字符串，直接返回。

### 重复

```javascript
const result = repeat(chars, Math.ceil(length / stringSize(chars)))
```

调用 `stringSize` 来取得 `chars` 的长度，再使用 `length` 除以 `chars` 的长度得到字符串的重复多少次才能达到 `length` 的长度，但是计算出来的次数是可能有小数的。

因此需要使用 `Math.ceil` 来向上取整，再调用 `repeat` 来重复得到结果 `result` ，因为是向上取整，这个 `result` 有可能比 `length` 要长。

### 截断

```javascript
return hasUnicode(chars)
    ? castSlice(stringToArray(result), 0, length).join('')
    : result.slice(0, length)
```

因为 `result` 有可能比 `length` 要长，因此需要对 `result` 进行截断操作。

截断需要区分两种情况，如果含有 `unicode` 编码，则先调用 `stringToArray` 将 `result` 转换成字符串数组，然后调用 `castSlice` 来进行截断，截断后就是一个长度为 `length` 的字符串数组，再用 `join` 方法拼接成长度为 `length` 的字符串，这样可以避免直接使用 `slice` 来截断出现乱码。

如果没有 `unicode` 编码，直接使用字符串的 `slice` 截断。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

