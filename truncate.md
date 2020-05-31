# lodash源码分析之truncate

本文为读 lodash 源码的第三百四十一篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseToString from './.internal/baseToString.js'
import castSlice from './.internal/castSlice.js'
import hasUnicode from './.internal/hasUnicode.js'
import isObject from './isObject.js'
import isRegExp from './isRegExp.js'
import stringSize from './.internal/stringSize.js'
import stringToArray from './.internal/stringToArray.js'
import toString from './toString.js'
```

[《lodash源码分析之baseToString》](./internal/baseToString.md)

[《lodash源码分析之castSlice》](./internal/castSlice.md)

[《lodash源码分析之hasUnicode》](./internal/hasUnicode.md)

[《lodash源码分析之isObject》](./isObject.md)

[《lodash源码分析之isRegExp》](./isRegExp.md)

[《lodash源码分析之stringSize》](./internal/stringSize.md)

[《lodash源码分析之stringToArray》](./internal/stringToArray.md)

[《lodash源码分析之toString》](./toString.md)

## 源码分析

`truncate` 的作用接收两个参数，一个是字符串 `string` 和 `options` 。

### options参数说明

* `options.length` ：结果字符串的长度，默认为 `30` 
* `options.omission`：省略符，如果字符串长度超过后，会将字符串截断，并接上省略符 ，默认为 `...`。
* `options.separator` ：截断点，可以为正则，也可以为字符串，在原字符串长度超出 `length` 时，不止是按长度来截断，还要在截断点处拼接上省略符 ，因此结果字符串可能会比 `length` 要小。

### 一些常量

```javascript
const DEFAULT_TRUNC_LENGTH = 30
const DEFAULT_TRUNC_OMISSION = '...'

const reFlags = /\w*$/
```

`DEFAULT_TRUNC_LENGTH` ：结果字符串默认长度为 `30`。

`DEFAULT_TRUNC_OMISSION` : 默认省略符为 `...` 。

`reFlags` 用来匹配正则的修饰符。

### 处理options

```javascript
let separator
let length = DEFAULT_TRUNC_LENGTH
let omission = DEFAULT_TRUNC_OMISSION

if (isObject(options)) {
  separator = 'separator' in options ? options.separator : separator
  length = 'length' in options ? options.length : length
  omission = 'omission' in options ? baseToString(options.omission) : omission
}
```

逻辑很简单，就是判断 `options` 上对应的属性有没有传入，如果没有传入则用默认值替代。

注意 `omission` 会使用 `baseToString` 将传入的值转换成 `string` 类型。

### 处理长度不足的字符串

```javascript
string = toString(string)

let strSymbols
let strLength = string.length
if (hasUnicode(string)) {
  strSymbols = stringToArray(string)
  strLength = strSymbols.length
}
if (length >= strLength) {
  return string
}
```

先将 `string` 使用 `toString` 来转换。

然后获取字符串的长度，获取字符串的长度需要检测字符串中是否有 `unicode` 编码，如果有，则使用 `syringToArray` 将字符串转换成数组 `strSymbols` ，再获取数组的长度。因此，当 `strSymbols` 有值时，也表示字符串中含有 `unicode` 编码。

接着目标长度 `length` 是否比 `strLength` 长，如果是，表示字符串没有超长，不需要做任何处理，直接将原字符串返回。

### 处理省略符超长的情况

```javascript
let end = length - stringSize(omission)
if (end < 1) {
  return omission
}
```

在需要做截断的情况下，需要计算截断的位置，截断的位置即为目标长度减去省略符的长度，得到的位置是 `end` 。

如果 `end` 小于 `1` ，即传入的省略符长度不比目标长度小，不需要再做截断处理，直接将省略符作为结果返回。

### 没有传截断点的情况

```javascript
let result = strSymbols
? castSlice(strSymbols, 0, end).join('')
: string.slice(0, end)

if (separator === undefined) {
  return result + omission
}
```

因为上面已经计算出截断的位置 `end` ，因此，对于含有 `unicode` 字符的字符串，使用 `castSlice` 将 `strSymbols` 中 `0` 到 `end` 位置的字符取出，因为得到的还是数组，因此再调用 `join` 方法得到截断后的字符串。

不含 `unicode` 编码的字符串，直接使用 `slice` 方法截取。

最后将 `result` 和省略符 `omission` 拼接在一起即可得到结果。

### 有传截断点的情况

如果有传截断点，则还需要找到截断点的位置，再在截断点处将字符串截断，再拼接省略符，还要保证拼接后的字符串不能超过原来的字符串。

#### 计算unicode字符串真正的截断索引

```javascript
if (strSymbols) {
  end += (result.length - end)
}
```

因为 `unicode` 编码的字符串的长度和索引是不对应的，因此先用结果的长度减去现在的截断点的长度，得到差值，再将截断点的长度加上差值，即可得到截断点的索引值，因为下面会使用到索引，这里先计算出来。

#### seperator 为正则的处理方式

```javascript
if (isRegExp(separator)) {
  if (string.slice(end).search(separator)) {
    let match
    let newEnd
    const substring = result

    if (!separator.global) {
      separator = RegExp(separator.source, `${reFlags.exec(separator) || ''}g`)
    }
    separator.lastIndex = 0
    while ((match = separator.exec(substring))) {
      newEnd = match.index
    }
    result = result.slice(0, newEnd === undefined ? end : newEnd)
  }
```

如果为正则，则使用字符串的 `search` 方法，来判断字符串到截断点索引的位置有没有匹配该正则的字符串。

如果没有，则不需要继续处理。

因为后面会使用 `exec` 来匹配，并且是全局匹配，因此判断传入的正则是否为 `global` 的，如果不是全局的，则先将正则转换成全局匹配。

可以看到，这里只能上面已经处理过的 `result` 进行匹配，因为 `result` 的长度加上省略符的的长度刚好是目标长度，因此只需要匹配 `result` 最后一个截断点即可，所以如果有截断点，则得到的结果可能会比目标长度要小。

使用 `exec` 方法来对 `result` 进行截断点的匹配，直至最后一个截断点，如果 `newEnd` 不为 `undefined` ，表示有截断点，再调用 `result` 的 `slice` 方法来进行截断。

#### seperator为字符串的情况

```javascript
else if (string.indexOf(baseToString(separator), end) != end) {
  const index = result.lastIndexOf(separator)
  if (index > -1) {
    result = result.slice(0, index)
  }
}
```

使用 `indexOf` 方法检测 `0` 到 `end` 范围内，`seperator` 的索引，如果索引值和 `end` 不相等，则表示可能有截断点，当然，如果没有截断点也是不相等的，因为得到的是 `-1` 。

接下来，使用 `lastIndexOf` 找出最后一个截断点在 `result` 中的位置，如果位置大于 `-1` ，表示有截断点，使用 `slice` 对 `result` 进行截断。

#### 最终结果

```javascript
return result + omission
```

在对 `result` 进行处理完毕后，拼接上省略符即可。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

