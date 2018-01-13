## 读lodash源码之从slice看稀疏数组与密集数组

> 卑鄙是卑鄙者的通行证，高尚是高尚者的墓志铭。
>
> ——北岛《回答》

看北岛就是从这两句诗开始的，高尚者已死，只剩卑鄙者在世间横行。

本文为读 lodash 源码的第一篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 引言

你可能会有点奇怪，原生的 slice 方法基本没有兼容性的问题，为什么 lodash 还要实现一个 slice  方法呢？

这个问题，lodash 的作者已经在 [why not the 'baseslice' func use Array.slice(), loop faster than slice?](https://github.com/lodash/lodash/issues/2850) 的 issue 中给出了答案：lodash 的 slice 会将数组当成密集数组对待，原生的 slice 会将数组当成稀疏数组对待。

## 密集数组VS稀疏数组

我们先来看看犀牛书是怎样定义稀疏数组的：

> 稀疏数组就是包含从0开始的不连续索引的数组。通常，数组的length属性值代表数组中元素的个数。如果数组是稀疏的，length属性值大于元素的个数。

如果数组是稀疏的，那么这个数组中至少有一个以上的位置不存在元素（包括 `undefined` ）。

例如：

```javascript
var sparse = new Array(10)
var dense = new Array(10).fill(undefined)
```

其中 `sparse` 的 `length` 为10，但是 `sparse` 数组中没有元素，是稀疏数组；而 `dense` 每个位置都是有元素的，虽然每个元素都为`undefined`，为密集数组 。

那稀疏数组和密集数组有什么区别呢？在 lodash 中最主要考虑的是两者在迭代器中的表现。

稀疏数组在迭代的时候会跳过不存在的元素。

```javascript
sparse.forEach(function(item){
  console.log(item)
})
dense.forEach(function(item){
  console.log(item)
})
```

`sparse` 根本不会调用 `console.log` 打印任何东西，但是 `dense` 会打印出10个 `undefined` 。

## 源码总览

当然，除了对待稀疏数组跟原生的 slice 不一致外，其他的规则还是一样的，下面是 lodash 实现 slice 的源码。

```javascript
function slice(array, start, end) {
  let length = array == null ? 0 : array.length
  if (!length) {
    return []
  }
  start = start == null ? 0 : start
  end = end === undefined ? length : end

  if (start < 0) {
    start = -start > length ? 0 : (length + start)
  }
  end = end > length ? length : end
  if (end < 0) {
    end += length
  }
  length = start > end ? 0 : ((end - start) >>> 0)
  start >>>= 0

  let index = -1
  const result = new Array(length)
  while (++index < length) {
    result[index] = array[index + start]
  }
  return result
}
```

## 不传参的情况

```javascript
let length = array == null ? 0 : array.length
if (!length) {
  return []
}
```

不传参时，`length` 默认为0，否则获取数组的长度。注意这里用的是 `array == null` ，非 `array === null` ，包含了 `undefined` 的判断。

所以在不传参调用 lodash 的 slice 时，返回的是空数组，而原生的 slice 没有这种调用方式。

## 处理start参数

`start` 参数用来指定截取的开始位置。

先来看下 MDN 对该参数的描述：

> 如果该参数为负数，则表示从原数组中的倒数第几个元素开始提取。
>
> 如果省略，则从索引0开始

```javascript
start = start == null ? 0 : start
```

因此这段是处理省略的情况，省略时，默认值为0。

```javascript
if (start < 0) {
  start = -start > length ? 0 : (length + start)
}
```

这段是处理负数的情况。

如果负数取反后比数组的长度还要大，即超出了数组的范围，则取值为0，表示从开始的位置截取，否则用 `length + start` ，即向后倒数。

```javascript
start >>>= 0
```

最后，用在 `>>>` 来确保 `start` 参数为整数或0。

因为 lodash 的 slice 除了可以处理数组外，也可以处理类数组，因此第一个参数 `array` 可能为一个对象， `length` 属性不一定为数字。

## 处理end参数

`end` 参数用来指定截取的结束位置。

同样来看下 MDN 对些的描述：

> 如果该参数为负数，则它表示在原数组中的倒数第几个元素结束制取。
>
> 如果end被省略，则slice会一直提取到原数组的末尾。
>
> 如果end大于数组长度，slice也会一直提取到原数组末尾。

```javascript
end = end === undefined ? length : end
```

这段是处理 `end` 被省略的情况，省略时，`end` 默认为为 `length`，即截取到数组的末尾。

```javascript
end = end > length ? length : end
```

这是处理 `end` 比数组长度大的情况，如果被数组长度大，也会截取到数组的末尾。

```javascript
if (end < 0) {
  end += length
}
```

这段是处理负值的情况，如果为负值，则从数组末尾开始向前倒数。

这里没有像 `start` 一样控制 `end` 的向前倒数完后是否为负数，因为后面还有一层控制。

## 获取新数组的长度

```javascript
length = start > end ? 0 : ((end - start) >>> 0)
```

新数组的长度计算方式很简单，就是用 `edn - start` 即可得出。

上面说到，没有控制最终 `end` 是否为负数的情况。这里用的是 `start` 和 `end` 的比较，如果 `start` 比 `end` 大，则新数组长度为0，即返回一个空数组。否则用 `end - start` 来计算。

这里同样用了无符号右移位运算符来确保 `length` 为正数或0。

## 截取并返回新数组

```javascript
let index = -1
const result = new Array(length)
while (++index < length) {
  result[index] = array[index + start]
}
return result
```

`result` 为新数组容器。

用 `while` 循环，从 `start` 位置开始，获取原数组的值，依次存入新的数组中。

因为是通过索引取值，如果遇到稀疏数组，对应的索引值上没有元素时，通过数组索引取值返回的是 `undefined`， 但这并不是说稀疏数组中该位置的值为 `undefined` 。

最后将 `result` 返回。

## 参考

1. javascript权威指南(第6版), David Flanagan著,淘宝前端团队译,机械工业出版社
2. [why not the 'baseslice' func use Array.slice(), loop faster than slice?](https://github.com/lodash/lodash/issues/2850)
3. [Array.prototype.slice()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)
4. [JavaScript: sparse arrays vs. dense arrays](http://2ality.com/2012/06/dense-arrays.html)
5. [[译]JavaScript中的稀疏数组与密集数组](http://www.cnblogs.com/ziyunfei/archive/2012/09/16/2687165.html)

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注，欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面