# lodash源码分析之isLength

> 如果我们不改变观点，仍然立足于“生”来探讨“死”，那么我们的思想永远只能停留在“生”的延长线上。
>
> ——青木新门《纳棺夫日记》

本文为读 lodash 源码的第二十四篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`isLength` 用来判断所传入的值是否为数组或者类数组可用的 `length` 属性。源码很简单。

```javascript
const MAX_SAFE_INTEGER = 9007199254740991
function isLength(value) {
  return typeof value == 'number' &&
    value > -1 && value % 1 == 0 && value <= MAX_SAFE_INTEGER
}
```

这里每个一判断都有特定的作用，我们一个一个来看。

`typeof value == 'number'` ，很明显，`length` 属性必须是 `number` 类型。

`value > -1` ，正常的数组下标是从 `0` 开始的，因此可以用大于 `-1` 来判断。

`value % 1 == 0` ，数组的下标必需是整数，这个可以与 `1` 取模来判断，这个条件和上一个条件合起来就是 `length` 必须为大于 `-1` 的整数。

`value <= MAX_SAFE_INTEGER` ，众所周知，`js` 所能表达的整数范围是有限的，`length` 是数字，也受这个限制，因此最后要判断其数值不能超过 `js` 所表达的最大正整数。

因此可以总结出来，是否为合法的数组 `length` 属性包含下面几个条件：

* 是数字
* 大于 `-1`
* 整数
* 数值在安全范围内

这几个条件的判断顺序也是经过精心安排的，越排在前面的，使用的频率越高，这样可以避免无效的判断。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 