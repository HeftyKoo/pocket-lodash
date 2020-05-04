# lodash源码分析之isWeakMap

本文为读 lodash 源码的第二百三十七篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import getTag from './.internal/getTag.js'
import isObjectLike from './isObjectLike.js'
```

[《lodash源码分析之数据类型获取的兼容性》](internal/getTag.md)

[《lodash源码分析之isObjectLike》](./isObjectLike.md)

## 源码分析

`isWeakMap` 用来检测 `value` 是否为 `WeakMap` 数据类型。

源码如下：

```javascript
function isWeakMap(value) {
  return isObjectLike(value) && getTag(value) == '[object WeakMap]'
}
```

先检测是否为类对象，再用 `getTag` 函数获取到 `value` 的 `Tag` ，看是否为 `[object WeakMap]`。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

