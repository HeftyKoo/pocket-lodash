# lodash源码分析之baseAt

本文为读 lodash 源码的第七十三篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import get from '../get.js'
```

[《lodash源码分析之get》](../get.md)

## 源码分析

`baseAt` 可以根据指定的一组属性路径 `paths`，从 `object` 中取出属性路径对应的一组值。

源码如下：

```javascript
function baseAt(object, paths) {
  let index = -1
  const length = paths.length
  const result = new Array(length)
  const skip = object == null

  while (++index < length) {
    result[index] = skip ? undefined : get(object, paths[index])
  }
  return result
}
```

首先初始化 `index = -1`，`length` 为路径数组的长度。

结果的数组 `result` 的长度肯定会和传入属性路径的数量一样，因此将结果初始化成长度为 `length` 的数组。

接下来，进行索引的遍历，因为是 `++index`，所以，在第一次遍历的时候，`index` 会为 `0` 。

如果 `object == null` ，即 `object` 为 `undefined` 或者 `null`，无论属性路径是什么，都是取不到值的，因此，在对应的位置赋值为 `undefined`。

如果有传递 `objecgt`，则调用 `get` 取出对应属性的路径的值赋值即可。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 