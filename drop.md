# lodash源码分析之drop

本文为读 lodash 源码的第三十一篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import slice from './slice.js'
```

[读lodash源码之从slice看稀疏数组与密集数组](./slice.md)

## 源码分析

`drop` 的作用是在移除数组中前 `n` 个数的元素，并将剩余的元素返回。如果指定的个数比数组的长度大，返回的是空数组。源码如下：

```javascript
function drop(array, n=1) {
    const length = array == null ? 0 : array.length
    return length
        ? slice(array, n < 0 ? 0 : n, length)
    : []
}
```

从源码中可以看出，如果没有指定个数，则默认移除1个元素。

如果 `array` 没有传递，返回的是空数组。

看源码可知，`drop` 其实调用的是 `slice` ，将 `n` 到 `array.length` 之间的元素返回。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 