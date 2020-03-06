# lodash源码分析之without

本文为读 lodash 源码的第一百零九篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseDifference from './.internal/baseDifference.js'
import isArrayLikeObject from './isArrayLikeObject.js'
```
[《lodash源码分析之数组的差集》](internal/baseDifference.md)
[《lodash源码分析之isArrayLikeObject》](isArrayLikeObject.md)

## 源码分析

`without` 的作用是剔除指定数组 `array` 中指定的值，剩余的值组成新的数组返回 。

其作用和 `pull` 相似，但是 `without` 不会更改原数组 `array` ，得到的是一个新的数组。

源码如下：

```javascript
function without(array, ...values) {
  return isArrayLikeObject(array) ? baseDifference(array, values) : []
}
```

`without` 接收不定参数，将除第一个参数外，需要剔除的值用 `...` 操作符组成一个数组 `values` 。

用 `isArrayLikeObject` 判断传入的是否为数组或者类数组，如果不是，返回一个空数组。

要剔除指定的值，其实就是要找出 `array` 和 `values` 的差集，因此使用 `baseDifference` 传入这两个数组即可。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 