# lodash源码分析之isBoolean

本文为读 lodash 源码的第二百零八篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import getTag from './.internal/getTag.js'
import isObjectLike from './isObjectLike.js'
```

《[lodash源码分析之数据类型获取的兼容性](./internal/getTag.md)》

《[lodash源码分析之isObjectLike](isObjectLike.md)》

## 源码分析

`isBoolean` 用来判断 `value` 是否为布尔值。

源码如下：

```javascript
function isBoolean(value) {
  return value === true || value === false ||
    (isObjectLike(value) && getTag(value) == '[object Boolean]')
}
```

布尔值只有两个，一个是 `true`，一个是 `false` 。

但是 `javascript` 可以用 `Boolean` 构造函数来创建布尔值，如果是用 `Boolean` 构造函数来创建，则得到的是一个对象，可以用 `getTag` 函数获取到类型字符串来进行比较。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 