# lodash源码分析之createMathOperation

本文为读 lodash 源码的第二百五十四篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseToNumber from './baseToNumber.js'
import baseToString from './baseToString.js'
```

[《lodash源码分析之baseToNumber》](./baseToNumber.md)

[《lodash源码分析之baseToString》](./baseToString.md)


## 源码分析

`createMathOperation` 的作用是用来规范 `Math` 类目下，各种方法的参数，避免每个方法都需要自己处理一遍。

源码如下：

```javascript
function createMathOperation(operator, defaultValue) {
  return (value, other) => {
    if (value === undefined && other === undefined) {
      return defaultValue
    }
    if (value !== undefined && other === undefined) {
      return value
    }
    if (other !== undefined && value === undefined) {
      return other
    }
    if (typeof value === 'string' || typeof other === 'string') {
      value = baseToString(value)
      other = baseToString(other)
    }
    else {
      value = baseToNumber(value)
      other = baseToNumber(other)
    }
    return operator(value, other)
  }
}
```

可以看到， `createMathOperation` 接收一个函数 `operator` 和一个默认值 `defaultValue` ，并且返回的也是一个函数，这个函数在处理完入参 `value` 和 `other` 后，最终调用的是 `operator` 得到结果。

### value和other同时为undefined

```javascript
if (value === undefined && other === undefined) {
  return defaultValue
}
```

两者同时为 `undefined` 时，直接返回默认值 `defaultValue` 即可。

### 只有other为undefined

```javascript
if (value !== undefined && other === undefined) {
  return value
}
```

如果只有 `other` 是 `undefined`，则只返回 `value` 即可。

### 只有value为undefined

```javascript
if (other !== undefined && value === undefined) {
  return other
}
```

只有 `value` 为 `undefined`，`other` 有值，则返回 `other`。

### other或value为string

```javascript
if (typeof value === 'string' || typeof other === 'string') {
  value = baseToString(value)
  other = baseToString(other)
}
```

只要 `other` 或者 `value` 其中之一为 `string` 类型，都将两者转换为 `string` 类型，再交由 `operator` 处理。

### 其它情况

```javascript
else {
  value = baseToNumber(value)
  other = baseToNumber(other)
}
```

其它情况下，将两者都转换为 `number` 类型，再交由 `operator` 处理。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

