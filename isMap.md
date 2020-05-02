# lodash源码分析之isMap

本文为读 lodash 源码的第二百二十四篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import getTag from './.internal/getTag.js'
import isObjectLike from './isObjectLike.js'
import nodeTypes from './.internal/nodeTypes.js'
```

[《lodash源码分析之getTag》](internal/getTag.md)

[《lodash源码分析之isObjectLike》](isObjectLike.md)

[《lodash源码分析之nodeTypes》](internal/nodeTypes.md)

## 源码分析

`isMap` 用来判断是否 `value` 为 `Map` 数据类型。

源码如下：

```javascript
const nodeIsMap = nodeTypes && nodeTypes.isMap
const isMap = nodeIsMap
  ? (value) => nodeIsMap(value)
  : (value) => isObjectLike(value) && getTag(value) == '[object Map]'

```

在 `node` 环境中，使用 `util.types` 的 `isMap` 方法来判断。

在其他环境中，先判断 `value` 是否为类对象类型，然后再通过 `getTag` 函数获取数据的 `Tag`，如果是 `[object Map]` 则为 `Map` 数据类型。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 