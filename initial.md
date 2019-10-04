# lodash源码分析之initial

本文为读 lodash 源码的第四十三篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import slice from './slice.js'
```

[《读lodash源码之从slice看稀疏数组与密集数组》](slice.md)

## 作用与用法

`initial` 的作用是返回一个新数组，这个新数组包括除原数组最后一项外的所有项。

```javascript
initial([1, 2, 3]) // [1, 2]
```

## 源码分析

```javascript
function initial(array) {
  const length = array == null ? 0 : array.length
  return length ? slice(array, 0, -1) : []
}
```

如果不考虑一些边缘情况，用 `slice(array, 0, -1)` 就可以达到 `initial` 的效果。

`initial` 处理了 `array` 为 `null` 或 `undefined` 的情况，在这种情况下，返回空数组。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 