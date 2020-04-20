# lodash源码分析之baseConformsTo

本文为读 lodash 源码的第二百零三篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)


## 源码分析

`baseConformsTo` 会对 `object` 上指定的属性值 `props` 都做检测，检测函数由 `source` 提供。

源码如下：

```javascript
function baseConformsTo(object, source, props) {
  let length = props.length
  if (object == null) {
    return !length
  }
  object = Object(object)
  while (length--) {
    const key = props[length]
    const predicate = source[key]
    const value = object[key]

    if ((value === undefined && !(key in object)) || !predicate(value)) {
      return false
    }
  }
  return true
}
```

### 处理null和undefined

```javascript
let length = props.length
if (object == null) {
  return !length
}
```

如果传入的 `object` 是 `null` 或者 `undefined` ，只要需要检测的属性值 `props` 长度不为零，得到的结果为 `false` 。因为 `null` 和 `undefined` 上根本没有属性可以检测。

### 遍历

先使用 `Object` 构造函数转换，确保基本类型也要转换成对象，避免出错。

然后遍历 `props`，分别从 `object` 中取出值 `value` ，从 `source` 中取出断言函数 `predicate` 。

如果 `value` 为 `undefined` 并且 `object` 上（包括原型链）上并不存在属性 `key`，那根本不需要调用断言函数了，因为要检测的属性在 `object` 上都不存在，可以直接返回 `false`，中止循环。

否则，将 `value` 传给断言函数 `predicate` ，如果得到假值，表示有属性没有通过断言函数的检测，返回的结果也为 `false`，中止循环。

如果所有属性都通过了对应断言函数的检测，则得到结果为 `true`。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 