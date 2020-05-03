# lodash源码分析之isRegExp

本文为读 lodash 源码的第二百三十四篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import getTag from './.internal/getTag.js'
import isObjectLike from './isObjectLike.js'
import nodeTypes from './.internal/nodeTypes.js'
```

《[lodash源码分析之数据类型获取的兼容性](./internal/getTag.md)》
《[lodash源码分析之isObjectLike](isObjectLike.md)》
《[lodash源码分析之nodeTypes](internal/nodeTypes.md)》

## 源码分析

`isRegExp` 用来检测 `value` 是否为正则。

源码如下：

```javascript
const nodeIsRegExp = nodeTypes && nodeTypes.isRegExp
const isRegExp = nodeIsRegExp
  ? (value) => nodeIsRegExp(value)
  : (value) => isObjectLike(value) && getTag(value) == '[object RegExp]'
```

在 `node` 环境中，使用 `util.types` 的 `isRegExp` 方法直接检测。

在其他环境中，使用 `getTag` 方法，看拿到的 `Tag` 是否为 `[object RegExp]` ，如果是，则为正则。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

