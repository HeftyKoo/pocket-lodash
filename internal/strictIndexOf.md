# lodash源码分析之strictIndexOf

> 你是否
>
> 偶尔也会低下头
>
> 望望那些从你道德的筛网中
>
> 不经意漏下去的良知
>
> ——79秒《网罟座·良知》

本文为读 lodash 源码的第十一篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

如果看过 `indexOf` 的规范，就会知道，数组原生方法 `indexOf` 采用的是 [`Strict Equality Comparison`](http://ecma-international.org/ecma-262/7.0/#sec-strict-equality-comparison) 的比较方式。

`strictIndexOf` 采用的比较方式与 `indexOf` 的一致，但是这个方法是 lodash 的内部方法，会在多个地方复用，在这里没有像 `indexOf` 一样处理 `fromIndex` 为负数的情况。

后面的文章将会介绍到，lodash 也实现了一个 `indexOf` 的函数，这个函数采用的是 [`SameValueZero`](http://ecma-international.org/ecma-262/7.0/#sec-samevaluezero) 的比较方式。而 `strictIndexOf` 是实现 `indexOf` 函数的基础。

关于这两种比较方式的差异，在前面的文章 [lodash源码分析之NaN不是NaN](../eq.md) 中也有的提及。

好了，接下来看源码吧：

```javascript
function strictIndexOf(array, value, fromIndex) {
  let index = fromIndex - 1
  const { length } = array

  while (++index < length) {
    if (array[index] === value) {
      return index
    }
  }
  return -1
}
```

首先，将 `fromIndex` 减少 `1`，这是为了抵消初始进入循环体时，就自增的影响。

接着就很简单了，比较数组中每个位置的值是否与 `value` 严格相等（`===`），如果严格相等，则返回对应的索引值，否则返回 `-1` 。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面