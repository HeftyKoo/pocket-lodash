# lodash源码分析之toFinite

本文为读 lodash 源码的第五十三篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import toNumber from './toNumber.js'
```

[《lodash源码分析之toNumber》](toNumber.md)

## 源码分析

从方法名上看，`toFinite` 的作用大体有两个：

1.  将 `value` 转换成 `number` 类型
2. 返回有限值

看看源码：

```javascript
function toFinite(value) {
  if (!value) {
    return value === 0 ? value : 0
  }
  value = toNumber(value)
  if (value === INFINITY || value === -INFINITY) {
    const sign = (value < 0 ? -1 : 1)
    return sign * MAX_INTEGER
  }
  return value === value ? value : 0
}
```

### 判断是否为falsely

```javascript
if (!value) {
  return value === 0 ? value : 0
}
```

`falsely` 的值全部转换成 `0` 。

### 转换成number

```javascript
value = toNumber(value)
```

只需要调用 `toNumber` 就可以将 `value` 转换成 `number` 类型了。

### 处理无限值

```javascript
if (value === INFINITY || value === -INFINITY) {
  const sign = (value < 0 ? -1 : 1)
  return sign * MAX_INTEGER
}
```

如果转换后的值为正无限值或者负无限值，则将其转换成 `js` 所能表达的最大的正负值。

### 处理NaN

```javascript
value === value ? value : 0
```

在 `js` 中，`NaN` 和 `NaN` 是不相等的，所以可以用这个来判断一个值是否为 `NaN` ，如果值为 `NaN` ，则返回 `0` 。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 