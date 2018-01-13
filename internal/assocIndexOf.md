# lodash源码分析之自减的两种形式

> 这个世界需要一个特定的恶人，可以供人们指名道姓，千夫所指：“全都怪你”。
>
> ——村上春树《当我谈跑步时我谈些什么》

本文为读 lodash 源码的第六篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

本篇分析的是 `assocIndexOf` 函数。

## 作用与用法

`assocIndexOf` 是 lodash 的内部函数，之前在《[lodash源码分析之Hash缓存](hash.md)》介绍过一种这样的数据结构：

```javascript
var caches = [['test1', 1],['test2',2],['test3',3]]
```

这是一个二维数组，每项中的第一项作为缓存对象的 `key`，第二项为缓存的值。

`assocIndexOf` 的作用是找出指定的 `key` 在数组中的索引值。

例如要找 `key` 为 `tes1` 的索引 ：

```javascript
assocIndexOf(caches, 'test1') // 0
```

## 依赖

```javascript
import eq from '../eq.js'
```

[lodash源码分析之NaN不是NaN](../eq.md)

## 源码分析

```javascript
function assocIndexOf(array, key) {
  let { length } = array
  while (length--) {
    if (eq(array[length][0], key)) {
      return length
    }
  }
  return -1
}
```

这段代码很精简，让 `length` 自减，调用 `eq` 函数，从二维数组的最后一项开始，逐项获取 `key` 值，与传入的 `key` 比较，遇到匹配的，马上将该项的索引返回。如果都没找到，返回 `-1` 。返回结果的规则与 `indexOf` 一致。

## length--和--length

我们都知道自减还有另外一种前置的形式，即 `--length`，那将上面的代码改成 `while(--length)` 可不可以呢？试一下就知道了。

改了之后，用 `caches` 来测试下：

```javascript
assocIndexOf(caches, 'test3') // 2
assocIndexOf(caches, 'test2') // 1
assocIndexOf(caches, 'test1') // -1
```

可以看到，改了之后，只影响到了第一项的结果，也就是终止条件有问题，根本没有遍历到第一项，但是后面的结果是正确的，也就说循环体里的 `length` 没有受到影响。

你可能会有点疑惑，`while` 的终止条件比较的不是 `length` 吗？为什么 `length--` 正确，而 `--length` 不正确呢？

其实 `while` 的终止条件并不是 `length` ，而是 `length--` 表达式所返回的结果。现在来看一下 `length--` 和 `--length` 所返回的结果有什么差别。

```javascript
var length = 3
length-- // 3
length // 2
```

可以看到， `length--` 返回的结果和自减前的一致，但是 `length` 已经减少 `1` 了。因此使用 `length--` ，最后一次进入循环体应该在 `length` 等于 `1` 的时候。

再来看 `--length`

```javascript
var length = 3
--length // 2
length // 2
```

`--length` 返回的结果跟自减后的结果一致，因此最后一次进入循环体应该是 `length` 为 `2` 的时候，因此如果换成这种形式，会漏掉一次循环。

##  参考

1. [代码之谜（二）- 语句与表达式](http://justjavac.com/codepuzzle/2012/10/28/codepuzzle-expression-and-statement.html)

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注，欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面