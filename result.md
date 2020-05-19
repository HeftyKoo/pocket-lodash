# lodash源码分析之result

本文为读 lodash 源码的第三百零一篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import castPath from './.internal/castPath.js'
import toKey from './.internal/toKey.js'
```

[lodash源码分析之castPath](./internal/castPath.md)

[lodash源码分析之toKey](./internal/toKey.md)

## 源码分析

`result` 的作用跟 `get` 差不多，但是当传入的路径 `path` 对应的值是函数时， `result` 会执行这个函数，并且 `this` 会绑定到它的父级对象。

源码如下：

```javascript
function result(object, path, defaultValue) {
  path = castPath(path, object)

  let index = -1
  let length = path.length

  if (!length) {
    length = 1
    object = undefined
  }
  while (++index < length) {
    let value = object == null ? undefined : object[toKey(path[index])]
    if (value === undefined) {
      index = length
      value = defaultValue
    }
    object = typeof value === 'function' ? value.call(object) : value
  }
  return object
}
```

### 处理path长度为0的情况

```javascript
path = castPath(path, object)

let index = -1
let length = path.length

if (!length) {
  length = 1
  object = undefined
}
```

先调用 `castPath` 来将 `path` 统一转换成路径数组的形式。

如果路径数组的长度为 `0` ，表示路径为空，这种情况下是要返回 `defaultValue` 的，但是看到这里仅仅是将 `length` 设置成 `1` 和将 `object` 设置成 `undefined` 。

这样做的原因是为了统一逻辑，返回 `defaultValue` 的会在下面处理。

### 遍历

```javascript
while (++index < length) {
  let value = object == null ? undefined : object[toKey(path[index])]
  if (value === undefined) {
    index = length
    value = defaultValue
  }
  object = typeof value === 'function' ? value.call(object) : value
}
```

遍历路径，在开始后续逻辑之前，先判断 `object` 是否为 `null` 或 `undefined` 值，如果是，则将 `value` 设置为 `undefined` ，否则从 `objecgt` 上将对应的属性值取出。

如果取出的 `value` 为 `undefined` ，则肯定再没有下一层的属性，因此将 `index` 设置为 `length` ，这样就不会再执行下一次循环，但是又会执行本次循环余下的逻辑。同时将 `value` 设置为 `defaultValue` ，这也是 `path` 的长度为 `0` 时，将 `object` 设置为 `undefined` 的原因。

接下来判断 `value` 是否为函数，如果是函数，则调用这个函数，并且将 `this` 绑定到 `object` ，这个 `object` 也就是它的父级对象。得到的结果赋值给 `object` ，如果不是函数， `value` 也赋值给 `object` ，这样就实现了一层一层地从 `object` 上取值，直到最后一层。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 