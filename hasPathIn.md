# lodash源码分析之hasPathIn

本文为读 lodash 源码的第三百七十四篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import castPath from './.internal/castPath.js'
import isArguments from './isArguments.js'
import isIndex from './.internal/isIndex.js'
import isLength from './isLength.js'
import toKey from './.internal/toKey.js'
```

[《lodash源码分析之castPath》](./internal/castPath.md)

[《lodash源码分析之isArguments》](./isArguments.md)

[《lodash源码分析之isIndex》](./internal/isIndex.md)

[《lodash源码分析之isLength》](./isLength.md)

[《lodash源码分析之toKey》](./internal/toKey.md)

## 源码分析

`hasPathIn` 用于检测 `object` 上是否存在路径 `path` ，和 `hasPath` 不同的是，`hasPathIn` 会检测原型链上的属性。

如：

```javascript
const object = create({ 'a': create({ 'b': 2 }) })
hasPath(object, 'a.b') // => false
hasPathIn(object, ['a', 'b']) // => true
```

源码如下：

```javascript
function hasPathIn(object, path) {
  path = castPath(path, object)

  let index = -1
  let { length } = path
  let result = false
  let key

  while (++index < length) {
    key = toKey(path[index])
    if (!(result = object != null && key in Object(object))) {
      break
    }
    object = object[key]
  }
  if (result || ++index != length) {
    return result
  }
  length = object == null ? 0 : object.length
  return !!length && isLength(length) && isIndex(key, length) &&
    (Array.isArray(object) || isArguments(object))
}

```

源码和 `hasPath` 基本一致，只不过在进行属性检测的时候，使用的是 `in` 操作符，会进行原型链属性的检测，`hasPath` 使用的是 `Object.hasOwnProperty` ，只进行自身属性的检测。

`hasPath` 分析见： [《lodash源码分析之hasPath》](./hasPath.md)

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

