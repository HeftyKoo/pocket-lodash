# lodash源码分析之invoke

本文为读 lodash 源码的第一百四十九篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import castPath from './.internal/castPath.js'
import last from './last.js'
import parent from './.internal/parent.js'
import toKey from './.internal/toKey.js'
```

[《lodash源码分析之castPath》](internal/castPath.md)
[《lodash源码分析之last》](last.md)
[《lodash源码分析之parent》](internal/parent.md)
[《lodash源码分析之toKey》](internal/toKey.md)

## 源码分析

`invoke` 会调用 `object` 指定 `path` 上的方法，可以指定一组参数 `args` ，在调用方法时传入。

源码如下：

```javascript
const hasOwnProperty = Object.prototype.hasOwnProperty
function invoke(object, path, args) {
  path = castPath(path, object)
  object = parent(object, path)
  const func = object == null ? object : object[toKey(last(path))]
  return func == null ? undefined : func.apply(object, args)
}
```

先用 `castPath` 将 `path` 转换成属性路径数组，然后使用 `parent` 函数取出 `path` 取出路径终点的父级的值。

因为已经取出终点父级的值，因此只需要用 `last` 取出终点的 `key` ，再从父级中取出对应 `key` 的值，即为需要调用的函数 `func` 。

取出 `func` 之后，如果 `func` 有值，则通过 `apply` 将 `args`  传入，并且函数的 `this` 绑定为 `object` ，这样和直接使用 `.` 来调用函数的结果一致。

这也是为什么要这样绕，不直接用 `baseGet` 或者 `get` 函数来取值的原因，因为要拿到路径 `path`终点父级的值 `object`，调用 `apply` 的时候可以绑定。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 