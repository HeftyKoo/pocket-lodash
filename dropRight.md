# lodash源码分析之dropRight

本文为读 lodash 源码的第三十二篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import slice from './slice.js'
```

[读lodash源码之从slice看稀疏数组与密集数组](./slice.md)

## 源码分析

`dropRight` 的作用和 `drop` 差不多，但是方向是从后往前，源码如下：

```javascript
function dropRight(array, n=1) {
  const length = array == null ? 0 : array.length
  return length ? slice(array, 0, n < 0 ? 0 : -n) : []
}
```

从源码中可以看出，`dropRight` 依然调用的是 `slice` 。只是第三个参数为负数，其实返回的就是数组中 `0` 到 `array.length - n` 之间的元素。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 