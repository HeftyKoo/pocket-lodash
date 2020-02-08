# lodash源码分析之stringToPath

本文为读 lodash 源码的第六十八篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import memoizeCapped from './memoizeCapped.js'
```

[《lodash源码分析之memoizeCapped》](./memoizeCapped.md)

## 源码分析

`stringToPath` 同样是 `lodash` 的内部方法，作用是将深层嵌套属性字符串转换成路径数组，即将类似于 `a.b.c` 这样的字符串，转换成 `['a', 'b', 'c'] 这样的数组，`方便 `lodash` 从数组中将属性一个一个取出，然后取值。

代码如下：

```javascript
const charCodeOfDot = '.'.charCodeAt(0)
const reEscapeChar = /\\(\\)?/g
const rePropName = RegExp(
  // Match anything that isn't a dot or bracket.
  '[^.[\\]]+' + '|' +
  // Or match property names within brackets.
  '\\[(?:' +
    // Match a non-string expression.
    '([^"\'][^[]*)' + '|' +
    // Or match strings (supports escaping characters).
    '(["\'])((?:(?!\\2)[^\\\\]|\\\\.)*?)\\2' +
  ')\\]'+ '|' +
  // Or match "" as the space between consecutive dots or empty brackets.
  '(?=(?:\\.|\\[\\])(?:\\.|\\[\\]|$))'
  , 'g')
const stringToPath = memoizeCapped((string) => {
  const result = []
  if (string.charCodeAt(0) === charCodeOfDot) {
    result.push('')
  }
  string.replace(rePropName, (match, expression, quote, subString) => {
    let key = match
    if (quote) {
      key = subString.replace(reEscapeChar, '$1')
    }
    else if (expression) {
      key = expression.trim()
    }
    result.push(key)
  })
  return result
})
```

### 处理以 `.` 开头的字符串

```javascript
const charCodeOfDot = '.'.charCodeAt(0)
const result = []
if (string.charCodeAt(0) === charCodeOfDot) {
  result.push('')
}
```

以 `.` 开头的字符串，`lodash` 会认定最外层的属性为空字符串。

注意这里用 `charCodeAt(0)` 来获取 `.` 的 `charCode`，再和 `string` 的 `0` 位的 `charCode` 作对比，这里最开始的时候是用正则 `/^\./` 来做匹配的，后来有个提交了个 `pr` 才最终使用 `charCode` 来比较，因为这样的性能更好。

### 处理常规路径

`lodash` 的这个正则相当复杂，主要处理三种情况。

首先看看 `a.b.c` 这种情况。在这种情况下，可以简化成下面的代码：

```javascript
string.replace(rePropName, (match) => {
  let key = match
  result.push(key)
})
```

这种情况下，是最简单的，只有 `match` 有值，回调函数会调用三次，`match` 的值分别为 `a`、`b`、`c`。直接将结果`push` 入 `result` 数组即可。

### 处理一般中括号取值

在数组取值的情况下，会写上中括号加上索引，如 `a[0].b` 这样的形式，转换成路径数组是 `['a', '0', 'b']` 。

在这种情况下，可以简化成这样的形式：

```javascript
string.replace(rePropName, (match, expression) => {
  let key = match
  else if (expression) {
    key = expression.trim()
  }
  result.push(key)
})
```

在匹配到索引 `0` 时，`match` 的结果是 `[0]`，但是这里 `expression` 的结果是 `0` ，因此 `key` 为 `0` 。

### 处理带引号的情况

中括号的情况下，有些人会习惯性地带上引号，如写成 `a['0'].b` 的形式，如果这个 `string` 是通过接口返回的，可以还会带上转义字符，如 `a[\'0\'].b`，在这种情况下，`quote` 会有值 `,` ，这时 `subString` 为在没转义的情况下为 `0`，在有转义符的情况下为 `\'0\'` ，因此需要用 `reEscapeChar` 正则将转义字符替换掉。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 