# lodash源码分析之NaN不是NaN

> 暗恋之纯粹，在于不求结果，完全把自己锁闭在一个单向的关系里面。
>
> ——梁文道《暗恋到偷窥》

本文为读 lodash 源码的第五篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

本篇分析的是 `eq` 函数。

## 作用与用法

`eq` 函数用来比较两个值是否相等。遵循的是  [SameValueZero](http://ecma-international.org/ecma-262/6.0/#sec-samevaluezero) 规范。

```javascript
var obj1 = {test: 1}
var obj2 = {test: 1}
var obj3 = obj1
_.eq(1,1) // true
_.eq(+0, -0) // true
_.eq(obj1, obj3) // true
_.eq(obj1, obj2) // false
_.eq(NaN, NaN) // false
```

## 几个比较规范

### SameValueNonNumber

这个规范规定比较的值 `x` 和 `y` 都不为 `Number` 类型，照抄规范如下：

1. `x` 的类型不为 `Number` 类型
2. `y` 的类型与 `x` 的类型一致
3. 如果 `x` 的类型为 `Undefined` ，返回 `true`
4. 如果 `x` 的类型为 `Null` ，返回 `true`
5. 如果 `x` 的类型为 `String`，并且 `x` 和 `y` 的长度及编码相同，返回 `true`，否则返回 `false`
6. 如果 `x` 的类型为 `Boolean` ，并且 `x` 和 `y` 同为 `true` 或同为`false` ，返回 `true`，否则返回 `false`
7. 如果 `x` 的类型为 `Symbol` ，并且 `x` 和 `y` 具有相同的 `Symbol` 值，返回 `true`，否则返回 `false`
8. 如果 `x` 和 `y` 指向同一个对象，返回 `true`， 否则返回 `false` 

### Strict Equality Comparison

js 中的全等（`===`）便是遵循这个规范，照搬规范如下：

1. 如果 `x` 和 `y` 的类型不同，返回 `false`
2. 如果 `x` 的为 `Number` 类型：
   * a. 如果 `x` 为 `NaN` ，返回 `false`
   * b. 如果 `y` 为 `NaN` ，返回 `false`
   * c. 如果 `x` 和 `y` 的数值一致，返回 `true`
   * d. 如果 `x` 为 `+0` 并且 `y` 为 `-0` ，返回 `true`
   * e. 如果 `x` 为 `-0` 并且 `y` 为 `+0` ，返回 `true`
   * f. 返回 `false`
3. 按照 [SameValueNonNumber](http://ecma-international.org/ecma-262/7.0/#sec-samevaluenonnumber) 的结果返回


### SameValue

规范如下：

1. 如果 `x` 和 `y` 的类型不同，返回 `false`
2. 如果 `x` 的类型为 `Number`
   * a. 如果 `x`  为 `NaN` 并且 `y` 为 `NaN` ，返回 `true`
   * b. 如果 `x` 为 `+0` 并且 `y` 为 `-0` ，返回 `false`
   * c. 如果 `x` 为 `-0` 并且 `y` 为 `+0` ， 返回 `false`
   * d. 如果 `x` 和 `y` 的数值一致，返回 `true`
   * e. 返回 `false`
3. 按照 [SameValueNonNumber](http://ecma-international.org/ecma-262/7.0/#sec-samevaluenonnumber) 的结果返回

### SameValueZero

这个是 `eq` 遵循的规范，如下：

1. 如果 `x` 和 `y` 的类型不同，返回 `false`
2. 如果 `x` 的类型为 `Number`
   * a. 如果 `x`  为 `NaN` 并且 `y` 为 `NaN` ，返回 `true`
   * b. 如果 `x` 为 `+0` 并且 `y` 为 `-0` ，返回 `true`
   * c. 如果 `x` 为 `-0` 并且 `y` 为 `+0` ， 返回 `true`
   * d. 如果 `x` 和 `y` 的数值一致，返回 `true`
   * e. 返回 `false`
3. 按照 [SameValueNonNumber](http://ecma-international.org/ecma-262/7.0/#sec-samevaluenonnumber) 的结果返回

小结：`SameValueNonNumber` 是基本，`Strict Equality Comparison` 、`SameValue` 和 `SameValueZero` 只是在对待 `+0`、`-0` 和 `NaN` 上有区别。

## 源码分析

来看下 `eq` 的源码：

```javascript
function eq(value, other) {
  return value === other || (value !== value && other !== other)
}
```

其实`eq` 的源码其实就只有这么一句。

既然 `eq` 遵循的是 `SameValueZero` 规范，那就将源码来拆解一下，看它是怎样符合规范的。

首先，看第一部分：

```javascript
value === other
```

就是这么一段，符合的是 `Strict Equality Comparison` 规范，通过对比可以发现， `Strict Equality Comparison` 和 `SameValueZero` 只在对待 `NaN` 上有区别。

`Strict Equality Comparison` 规定就算 `x` 和 `y` 都为 `NaN` 时，返回的是 `false`， `NaN === NaN` 返回的就是 `false`。但是 `SameValueZero` 返回的是规定 `x` 和 `y` 都为 `NaN` 时返回的是 `true`。因此只需要在 `Strict Equality Comparison` 的基础上处理 `NaN` 就可以了。

下面这段便是处理 `NaN` 的：

```javascript
(value !== value && other !== other)
```

在 js 中，只有 `NaN` 和自身是不相等的，当两个需要比较的值都是和自身不相等时，表明这两个值都为 `NaN`，返回 `true`。

这样便遵循了 `SameValueZero` 的比较实现。

## 可以用Object.is()吗？

`Object.is(NaN, NaN)` 返回的是 `true` ，所以 `eq` 同样可以改成：

```javascript
function eq(value, other) {
  return value === other || Object.is(value, other)
}
```

`Object.is` 同样是比较两个值是否一样，但是 `Object.is(+0, -0)` 返回的是 `false`， 它遵循是的 `SameValue` 规范，因此不可以直接用 `Object.is` 替代 `eq` 。

## 可以用isNaN()吗？

还有个 `isNaN` 的全局方法，可以用来判断一个值是否为 `NaN`。例如 `isNaN(NaN)` 会返回 `true` ，那 `eq` 是否可以改成以下形式呢？

```javascript
function eq(value, other) {
  return value === other || (isNaN(value) && isNaN(other))
}
```

答案是：不可以！

`isNaN` 有一个很怪异的行为，如果传入的参数不为 `Number` 类型，会尝试转换成 `Number` 类型之后再做是否为 `NaN` 的判断。所以类似 `isNaN('notNaN')` 返回的也是 `true` ，因为字符串 `notNaN` 会先被转换成 `NaN` 再做判断，这不是我们想要的结果。

## 可以用Number.isNaN()吗

为了修复 `isNaN` 的缺陷，`es6` 在 `Number` 对象上扩展了 `isNaN` 方法，只有是 `NaN` 时才会返回 `true`，因此用 `Number.isNaN` 来判断是安全的。所以 `eq` 同样可以改成以下形式：

```javascript
function eq(value, other) {
  return value === other || (Number.isNaN(value) && Number.isNaN(other))
}
```

## 参考

1. [ECMAScript® 2016 Language Specification](http://ecma-international.org/ecma-262/7.0/#sec-samevaluenonnumber)
2. [MDN:Number.isNaN()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number/isNaN)
3. [MDN:isNaN()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/isNaN)

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注，欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面

