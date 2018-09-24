# lodash源码分析之isArrayLikeObject

> 诗歌是语言的花朵，任何怒放的花都会随着时间凋零，而花粉的种子也就在空间里随风四散，可能他们就躲在任何不为人知的土地的角落里，静静的等待另一个春天的来临。
>
> ——罗大佑

本文为读 lodash 源码的第二十六篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import isArrayLike from './isArrayLike.js'
import isObjectLike from './isObjectLike.js'
```

[《lodash源码分析之isArrayLike》](./isArrayLike.md)

[《lodash源码分析之isObjectLike》](./isObjectLike.md)

## 源码分析

```javascript
function isArrayLikeObject(value) {
  return isObjectLike(value) && isArrayLike(value)
}
```

`isArrayLikeObject` 其实是 `isObjectLike` 和 `isArrayLike` 的组合，需要检测的值必须要满足 `isObjectLike` 和 `isArrayLike` 的检测。

在 `isArrayLike` 函数中，像字符串也会被当作是类数组，但是在 `isArrayLikeObject` 中，`value` 的类型的 `typeof` 返回值必须是 `object` ，因为 `value` 要先经过 `isObjectLike` 检测。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 