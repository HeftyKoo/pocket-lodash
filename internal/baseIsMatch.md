# lodash源码分析之baseIsMatch

本文为读 lodash 源码的第二百二十七篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import Stack from './Stack.js'
import baseIsEqual from './baseIsEqual.js'
```

[《lodash源码分析之Stack》](./Stack.md)
[《lodash源码分析之baseIsEqual》](./baseIsEqual.md)

## 源码分析

`baseIsMatch` 是实现 `isMatch` 方法的基础，`isMath` 的大部分逻辑由 `baseIsMath` 来实现。

`baseIsMath` 用来检测 `object` 是否包含 `source` ，即 `object` 要包含 `source` 的所有属性，并且对应属性的值相等。

### 一些常量

```javascript
const COMPARE_PARTIAL_FLAG = 1
const COMPARE_UNORDERED_FLAG = 2
```

`COMPARE_PARTIAL_FLAG` 表示开启局部比较，`COMPARE_UNORDERED_FLAG` 表示开启无序比较。

### 空值处理

```javascript
let index = matchData.length
const length = index
const noCustomizer = !customizer

if (object == null) {
  return !length
}
```

`matchData` 在上篇文章[《lodash源码分析之getMatchData》](./getMatchData.md)已经有过分析，是 `source` 的 `[key, value, isStrictComparableFlag]` 集合。

因此 `matchData.length` 为 `0` 时，表示 `source` 是一个空对象。

如果 `object == null` ，表示 `object` 为 `null` 或者 `undefined`，此时，如果 `matchData.length` 为 `0` ，则返回的结果为 `true` ，否则为 `false`。

### 可全等比较值的比较

```javascript
let data
let result
object = Object(object)
while (index--) {
  data = matchData[index]
  if ((noCustomizer && data[2])
      ? data[1] !== object[data[0]]
      : !(data[0] in object)
     ) {
    return false
  }
}
```

遍历 `matchData`，`data[2]` 为 `true` 时，也即可使用 `===` 比较，并且 `noCustomizer` 为 `true` ，也即没有传入自定义比较函数时，使用以下的方式先进行检测：

```javascript
data[1] !== object[data[0]]
```

其中 `data[1]` 为 `value` ，`data[0]` 为 `key` ，这句的意思是，如果 `source` 中对应 `key` 的 `value` 和 `object` 对应 `key` 的 `value` 不相等，则 `object` 是不包含 `source` 的。

否则，在这步，再使用以下的方式检测：

```javascript
!(data[0] in object)
```

即存在于 `source` 中的 `key` 在 `object` 中不存在，`object` 也是不包含 `source` 的。

这两个比较都没有涉及到 `object` 包含 `source` 的情况，之所以要分开处理，是出于性能的考虑，只通过一层比较就可以排除掉一些不包含的情况。

### 处理object上不存在key的情况

```javascript
while (++index < length) {
  data = matchData[index]
  const key = data[0]
  const objValue = object[key]
  const srcValue = data[1]

  if (noCustomizer && data[2]) {
    if (objValue === undefined && !(key in object)) {
      return false
    }
  } else {
    ...
  }
}
```

接下来再遍历 `matchData`，再排除 `key` 在 `source` 上，但不在 `object` 上的情况。

在上面满足 `noCustomizer && data[2]` 这个条件时，使用的是 `===` 比较，如果 `key` 不在 `object` 上拿到的值会是 `undefined`，如果 `source` 的值恰好也是 `undefined`，得到的结果会是 `true` 。

因此这里使用 `key in object` 来检测，如果不在 `object` 上，则可以直接返回 `false` 。

### 自定义比较函数

```javascript
while (++index < length) {
  ...
  if (noCustomizer && data[2]) {
    ...
  } else {
    const stack = new Stack
    if (customizer) {
      result = customizer(objValue, srcValue, key, object, source, stack)
    }
    ...
  }
}
```

如果传入了自定义比较函数 `customizer` ，则直接调用 `customizer` 函数得出结果。

### baseIsEqual比较值是否相等

```javascript
if (!(result === undefined
      ? baseIsEqual(srcValue, objValue, COMPARE_PARTIAL_FLAG | COMPARE_UNORDERED_FLAG, customizer, stack)
      : result
     )) {
  return false
}
```

如果没有传入自定义比较函数，或者自定义比较函数返回的结果是 `undefined` ，则使用 `baseIsEqual` 来比较两者的值是否相等。

要注意这里 `bitmask` 传入的是局部比较和无序比较，因为是检测 `object` 是否包含 `source` ，使用局部比较即可。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 