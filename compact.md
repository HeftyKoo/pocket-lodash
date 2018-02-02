# lodash源码分析之compact中的遍历

>小时候，
>
>乡愁是一枚小小的邮票，
>
>我在这头，
>
>母亲在那头。
>
>长大后，乡愁是一张窄窄的船票，
>
>我在这头，
>
>新娘在那头。
>
>后来啊，
>
>乡愁是一方矮矮的坟墓，
>
>我在外头，
>
>母亲在里头。
>
>而现在，
>
>乡愁是一湾浅浅的海峡，
>
>我在这头，
>
>大陆在那头。
>
>——余光中《乡愁》

本文为读 lodash 源码的第三篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 作用与用法

`compact` 函数用来去除数组中的假值，并返回由不为假值元素组成的新数组。

`false`、`null`、`0`、 `""`、`undefined` 和 `NaN` 都为假值。

例如：

```javascript
var arr = [1,false,2,null,3,0,4,NaN,5,undefined]
_.compact(arr) // 返回 [1，2，3，4，5]
```

## 源码

```javascript
function compact(array) {
  let resIndex = 0
  const result = []

  if (array == null) {
    return result
  }

  for (const value of array) {
    if (value) {
      result[resIndex++] = value
    }
  }
  return result
}
```

`compact` 的源码只有寥寥几行，相当简单。

首先判断传入的数组是否为 `null` 或者 `undefined`，如果是，则返回空数组。

然后用 `for...of` 来取得数组中每项的值，如果不为假值，则存入新数组 `result` 中，最后将新数组返回。

到这里，源码分析完了。

但是在看源码的时候，发现这里用了 `for...of` 来做遍历，其实除了 `for...of` 外，也可以用 `for` 或者 `for...in` 来做遍历，那为什么最后选了 `for...of` 呢？

## 数组中的for循环

使用 `for` 循环，很容易就将 `compact` 中关于循环部分的源码改写成以下形式：

```javascript
for (let i = 0; i < array.length; i++) {
  	const value = array[i]
    if (value) {
      result[resIndex++] = value
    }
  }
```

这样写，肯定是没有问题的，但是不够简洁。

## for…in

再来看 `for...in` 循环，先来将源码改写一下：

```javascript
for (let index in array) {
  const value = array[index]
  if (value) {
    result[resIndex++] = value
  }
}
```

先看看MDN上关于 `for...in` 的用法：

> **for...in语句**以任意顺序遍历一个对象的[可枚举属性](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Enumerability_and_ownership_of_properties)。

关于可枚举属性，可以点击上面的链接到MDN上了解一下，这里不做太多的解释。

在数组中，数组的索引是可枚举属性，可以用 `for...in` 来遍历数组的索引，数组中的稀疏部分不存在索引，可以避免用 `for` 循环造成无效遍历的弊端。

但是，`for...in` 有两个致命的特性：

1. `for...in` 的遍历不能保证顺序
2. `for...in` 会遍历所有可枚举属性，包括继承的属性。

`for...in` 的遍历顺序依赖于执行环境，不同执行环境的实现方式可能会不一样。单凭这一点，就断然不能在数组遍历中使用 `for...in`，大多数情况下，顺序对于数组的遍历都相当重要。

关于第二点，先看个例子：

```javascript
var arr = [1,2,3]
arr.foo = 'foo'
for (let index in arr) {
  console.log(index)
}
```

在这个例子中，你期望输出的是 `0,1,2`，但是最后输出的可能是 `0,1,2,foo` （`for...in` 不能保证顺序）。因为 `foo` 也是可枚举属性，在 `for..in` 会被遍历出来。

## for…of

最后来看看 `for...of`。

当我们在控制台中打印一个数组，并将它展开来查看时，会在数组的原型链上发现一个很特别的属性 `Symbol.iterator`。

其实 `for...of` 循环内部调用的就是数组原型链上的 `Symbol.iterator` 方法。

`Symbol.iterator` 在调用的时候会返回一个遍历器对象，这个遍历器对象中包含 `next` 方法，`for...of` 在每次循环的时候都会调用 `next` 方法来获取值，直到 `next` 返回的对象中的 `done`属性值为 `true` 时停止。

其实我们也可以手动调用来模拟遍历的过程：

```javascript
const arr = [1,2,3]
const iterator = a[Symbol.iterator]()
iterator.next() // {value: 1, done: false}
iterator.next() // {value: 2, done: false}
iterator.next() // {value: 3, done: false}
iterator.next() // {value: undefined, done: true}
```

知道这些原理后，完全可以改写数组中的 `Symbol.iterator` 方法，例如遍历时将数组中的值都乘2：

```javascript
Array.prototype[Symbol.iterator] = function () {
  let index = 0
  const _self = this
  return {
    next: function () {
      if (index < _self.length) {
        return {value: _self[index++] * 2, done: false}
      } else {
        return {done: true}
      }
    }
  }
}
```

使用 `Generator` 函数可以写成以下的形式：

```javascript
Array.prototype[Symbol.iterator] = function* () {
  let index = 0
  while (index < this.length) {
    yield this[index++] * 2   
  }
}
```

因此在不改写 `Symbol.iterator` 的情况下，使用 `for...of` 来遍历数组是安全的，因为这个方法是数组的原生方法。

关于 `Iterator` 和 `Generator` 可以点击[参考](#参考)中的链接详细查看。

## 参考

1. [MDN:迭代器和生成器](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Iterators_and_Generators#Iterables)
2. [Iterator 和 for...of 循环](http://es6.ruanyifeng.com/#docs/iterator)
3. [Generator 函数的语法](http://es6.ruanyifeng.com/#docs/generator)
4. [Lodash源码讲解(3)-compact函数](https://dreamapple.me/2017/08/18/lodash%E6%BA%90%E7%A0%81%E8%AE%B2%E8%A7%A3-3/)
5. [MDN:for...of](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...of)
6. [MDN:for…in](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...in)

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注，欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面