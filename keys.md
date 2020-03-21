# lodash源码分析之keys

本文为读 lodash 源码的第一百三十篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import arrayLikeKeys from './.internal/arrayLikeKeys.js'
import isArrayLike from './isArrayLike.js'
```

[《lodash源码分析之arrayLikeKeys》](internal/arrayLikeKeys.md)
[《lodash源码分析之isArrayLike》](./isArrayLike.md)

## 源码分析

`keys` 函数用来收集 `object` 自身的可枚举属性。

如果传入的类型为非 `object` 类型，会使用 `Object` 构造函数转换成 `object` 类型。

源码如下：

```javascript
function keys(object) {
  return isArrayLike(object)
    ? arrayLikeKeys(object)
    : Object.keys(Object(object))
}
```

如果传入的数组是类数组，则使用 `arrayLikeKeys` 收集属性。

否则使用 `Object.keys` 收集属性，在收集的时候，会使用 `Object` 构造函数进行转换，避免传入非 `object` 类型。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 