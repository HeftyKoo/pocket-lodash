# lodash源码分析之maxBy

本文为读 lodash 源码的第二百六十篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import isSymbol from './isSymbol.js'
```

[lodash源码分析之isSymbol](./isSymbol.md)

## 源码分析

`maxBy` 的作用是找出一组数据中的最大值。和 `Math.max` 不同的是，`maxBy` 支持传入一个迭代器 `iteratee` ，在比较的过程中不直接比较原始值，而是使用迭代器返回的值进行比较。

源码如下：

```javascript
function maxBy(array, iteratee) {
  let result
  if (array == null) {
    return result
  }
  let computed
  for (const value of array) {
    const current = iteratee(value)

    if (current != null && (computed === undefined
      ? (current === current && !isSymbol(current))
      : (current > computed)
    )) {
      computed = current
      result = value
    }
  }
  return result
}
```

以下的代码用来处理空值：

```javascript
let result
if (array == null) {
  return result
}
```

如果没有传入 `array` 或者传入的是 `null` ，则直接返回 `result` ，此时 `result` 还没有赋值，也即为 `undefined` 。

 接着定义一个 `computed` 来保存上一轮比较完成后的较大值，这个值是 `iteratee` 的返回值，`computed` 在开始比较前是 `undefined`。

然后开始遍历 `array` ，每次遍历时，都调用迭代器 `iteratee` ，将当前 `value` 传给迭代器，得到计算结果 `current` 。

接下来是一个复杂的判断条件：

```javascript
current != null && (computed === undefined
                    ? (current === current && !isSymbol(current))
                    : (current > computed)
                   )
```

最先要满足的条件是 `current != null` ，即返回的结果如果是 `undefined` 或者 `null` ，则完全没有比较的必要。

接着再看 `computed === undefined` ，如果 `computed` 为 `undefined` ，则表示上一轮的比较没有结果，或者是则开始第一轮。这时候，只需要检测当前值即可，因为还没有可与之比较的值。

在这种情况下，当前值需要满足以下条件：

```javascript
current === current && !isSymbol(current)
```

`current === current` 表示 `current` 不能为 `NaN` ，因为 `NaN` 是无法比较大小的，也不能是 `Symbol` 类型，因为如果是 `Symbol` 类型，在隐式转换成 `number` 来比较大小时，会报错。

如果这两个条件都满足，则将 `computed` 设置为 `current` ，再与下一轮的计算值比较大小，`result` 也初始化为当前的 `value` 。

在 `computed` 不为 `undefined` 的时候，则表示上一轮的较大值为 `computed` ，此时要拿 `current` 和 `computed` 比较，如果满足 `current > computed` 时，即当前值比上一轮的较大值大时，则更新 `computed` 和 `result` 。

因此，当遍历完毕时，就会得到整个数组中的最大值 `result` 。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

