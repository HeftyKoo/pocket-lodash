# lodash源码分析之isArrayLike

> 有人认为所谓的开悟就是准备好了在任何时候都能从容死去，这其实是错误的。真正的开悟，是在任何情形下都能从容地活着。
>
> ——正冈子规《病床六尺》

本文为读 lodash 源码的第二十五篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import isLength from './isLength.js'
```

《[lodash源码分析之isLength](./isLength.md)》

## 源码分析

`isArrayLike` 用来判断传入的值是否为类数组，源码很简单：

```javascript
function isArrayLike(value) {
  return value != null && typeof value != 'function' && isLength(value.length)
}
```

 从源码中可以看出，lodash 对类数组的概念判定很宽松，只要满足以下三个条件即可：

* 不为 `null`
* 不是 `function`
* `length` 属性必须通过 `isLength` 函数的检测

第一个条件不讲，第三个条件在上一篇文章《[lodash源码分析之isLength](./isLength.md)》中已经讲清楚，现在来分析一下第二个条件。

为什么要判断值是不是函数呢？其实是因为函数上也会有 `length` 的属性，用来表示函数的形参个数。如：

```javascript
function test (param) {
    console.log(param)
}
test.length  // 1
```

因此在使用 `isArrayLike` 时，传入字符串返回的也会是 `true`，甚至传入 `window` 对象返回的也是 `true` 。因为 `window` 对象上有个 `length` 的属性，来表示 `frame` 和 `iframe` 的个数。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 