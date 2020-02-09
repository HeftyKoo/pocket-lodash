# lodash源码分析之baseGet

本文为读 lodash 源码的第七十一篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import castPath from './castPath.js'
import toKey from './toKey.js'
```

[《lodash源码分析之castPath》](./castPath.md)

[《lodash源码分析之toKey》](./toKey.md)

## 源码分析

`baseGet` 可以指定类似于这样的属性路径 `a.b.c` 深层取值。

源码如下：

```javascript
function baseGet(object, path) {
  path = castPath(path, object)

  let index = 0
  const length = path.length

  while (object != null && index < length) {
    object = object[toKey(path[index++])]
  }
  return (index && index == length) ? object : undefined
}
```

首先调用 `castPath` 将属性路径转换成类似于 `['a','b', 'c']` 路径数组。

然后循环数组，从 `object` 上一层一层取值。

如果最后 `index == length`，则表明取值已经到了指定的层级，直接返回取到的值 `object` ，否则中间取值会遇到 `undefined` 或者 `null` 的情况，这里返回 `undefined`。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 