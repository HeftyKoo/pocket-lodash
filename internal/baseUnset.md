# lodash源码分析之baseUnset

本文为读 lodash 源码的第七十五篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import castPath from './castPath.js'
import last from '../last.js'
import parent from './parent.js'
import toKey from './toKey.js'
```

[《lodash源码分析之castPath》](./castPath.md)

[《lodash源码分析之last》](../last.md)

[《lodash源码分析之parent》](./parent.md)

[《lodash源码分析之toKey》](./toKey.md)

## 源码分析

`baseUnset` 的作用是删除 `object` 中，指定属性路径 `path` 对应的值。删除成功返回 `true`。其中 `path` 可以是数组，也可以是属性路径字符串。

即可以是 `['a', 'b', 'c']`，也可以是 `a.b.c`。

源码如下：

```javascript
function baseUnset(object, path) {
  path = castPath(path, object)
  object = parent(object, path)
  return object == null || delete object[toKey(last(path))]
}
```

首先调用 `castPath` 将属性路径规范化成数组的形式。

然后调用 `parent` 取出当前路径的父级的值。

如果父级的值为 `null` ，则表示这个属性路径下没有值，不需要删除，直接返回 `object == null` ，这种情况下，返回的值为 `true` 。

否则调用 `last` 取出属性路径的最后的属性，因为之前已经取出父级的值 `object` ，因此只需要对父级进行 `delete` 操作即可，也即 `delete object[tokey(last(path))]` 。其中 `toKey` 对属性进行一些规范化的转换。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 