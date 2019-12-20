# lodash源码分析之isIndex

本文为读 lodash 源码的第五十六篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`isIndex` 的作用是用来判断传入的 `value` 是否为合法的数组或者类数组索引。

### 几个常量

```javascript
/** Used as references for various `Number` constants. */
const MAX_SAFE_INTEGER = 9007199254740991

/** Used to detect unsigned integer values. */
const reIsUint = /^(?:0|[1-9]\d*)$/
```

`MAX_SAFE_INTEGER` 是最大安全整数，因为 `javascript` 使用的是双精度浮点数，能安全表示的最大值为 `9007199254740991` 。

`reIsUint` 用来判断值是否带符号。即是否是 `+19` 或 `-19` 这样的形式。

### length值判断

`isIndex` 的整体源码如下：

```javascript
function isIndex(value, length) {
  const type = typeof value
  length = length == null ? MAX_SAFE_INTEGER : length

  return !!length &&
    (type === 'number' ||
      (type !== 'symbol' && reIsUint.test(value))) &&
        (value > -1 && value % 1 == 0 && value < length)
}
```

它的第二个参数 `length` 可以指定合法 `index` 的范围，如果没有指定的时候，默认的范围为最大安全整数 `MAX_SAFE_INTEGER`。

如果指定的 `length` 不存在或者为 `0`，则肯定没有合法的 `index` 值。

### 对value的type判断

```javascript
(type === 'number' ||
      (type !== 'symbol' && reIsUint.test(value)))
```

可以看到，`value` 的 `type` 要么是 `number` 类型，或者 `resIsUint.test(value)` 为 `true` 。这句代码很讲究，此时的 `value` 可以是除了 `number` 外的其他类型，也即可能是 `string`、`object`、`symbol` 等等，用正则的 `test` 方法，会将 `value` 隐式转换为 `string`，再判断是否符合 `resIsUint` 的正则。这就是为什么要排除 `symbol` 的原因了，因为规范中规则 `symbol` 在隐式转换为 `string` 或 `number` 时，会抛出 `TypeError` 错误，具体见参考资料中的 `ECMA规范` 。

### 对value 范围判断

```javascript
value > -1 && value % 1 == 0 && value < length
```

`value` 通过隐藏转换成 `number` 类型后，要满足三个条件才是合法的 `index` 。

* `value > -1` 
* `value % 1 === 0` ，能被 `1` 整除，和第一个条件合起来，即表示 `value` 要为自然数
* `value < length` ，即比指定的范围要小

## 参考资料

[ECMA规范](http://www.ecma-international.org/ecma-262/7.0/index.html#sec-tostring)

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 