# lodash源码分析之split

本文为读 lodash 源码的第三百三十三篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import castSlice from './.internal/castSlice.js'
import hasUnicode from './.internal/hasUnicode.js'
import isRegExp from './isRegExp.js'
import stringToArray from './.internal/stringToArray.js'
```

[《lodash源码分析之castSlice》](./internal/castSlice.md)

[《lodash源码分析之hasUnicode》](./internal/hasUnicode.md)

[《lodash源码分析之isRegExp》](./isRegExp.md)

[《lodash源码分析之stringToArray》](./internal/stringToArray.md)

## 源码分析

`split` 的作用基于字符串的 `split` 方法，会对 `unicode` 编码做特殊的处理。

源码如下：

```javascript
const MAX_ARRAY_LENGTH = 4294967295
function split(string, separator, limit) {
  limit = limit === undefined ? MAX_ARRAY_LENGTH : limit >>> 0
  if (!limit) {
    return []
  }
  if (string && (
    typeof separator === 'string' ||
        (separator != null && !isRegExp(separator))
  )) {
    if (!separator && hasUnicode(string)) {
      return castSlice(stringToArray(string), 0, limit)
    }
  }
  return string.split(separator, limit)
}
```

### 对limit处理

```javascript
limit = limit === undefined ? MAX_ARRAY_LENGTH : limit >>> 0
if (!limit) {
  return []
}
```

如果 `limit` 为 `undefined` ，则默认 `limit` 为最大的数组长度。

如果有传 `limit` ，则使用无符号位右移，将 `limit` 转换成正整数。

这里的转换很巧妙，用了无符号位右移，无符号位右移首先会忽略小数位，这样 `limit` 就变成整数了，然后这里右移的位数是 `0` 位，对于正整数来说，右移 `0` 位和原来的一样，对于负数来说，无符号位右移会将符号位变成 `0` ，右移 `0` 位就相当于取绝对值了。

如果转换后 `limit` 为 `0` ，则返回一个空数组。

### 处理含unicode字符串

```javascript
if (string && (
  typeof separator === 'string' ||
  (separator != null && !isRegExp(separator))
)) {
  if (!separator && hasUnicode(string)) {
    return castSlice(stringToArray(string), 0, limit)
  }
}
```

也不是所有含有 `unicode` 的字符串都需要特殊处理的，只有 `separator` 为假值时，这时如果直接使用 `split` 来分割，可能会出现乱码。

因此先用 `stringToArray` 将字符串转换成数组，然后用 `castSlice` 来截取规定的长度。

### 其它情况处理

```javascript
return string.split(separator, limit)
```

其它情况就很简单了，交由字符串的 `split` 方法处理即可。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

