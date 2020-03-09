# lodash源码分析之assignValue

本文为读 lodash 源码的第一百一十六篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseAssignValue from './baseAssignValue.js'
import eq from '../eq.js'
```

[《lodash源码分析之baseAssignValue》](./baseAssignValue.md)
[《lodash源码分析之NaN不是NaN》](../eq.md)

## 源码分析

`assignValue` 的作用是将 `value` 设置到 `object` 指定的 `key` 上。

源码如下：

```javascript
const hasOwnProperty = Object.prototype.hasOwnProperty
function assignValue(object, key, value) {
  const objValue = object[key]

  if (!(hasOwnProperty.call(object, key) && eq(objValue, value))) {
    if (value !== 0 || (1 / value) === (1 / objValue)) {
      baseAssignValue(object, key, value)
    }
  } else if (value === undefined && !(key in object)) {
    baseAssignValue(object, key, value)
  }
}
```

要将 `value` 设置到指定的 `key` 上很简单，只要检测一下 `object` 上是否有这个 `key` ，如果这个 `key` 不存在，或者原来的值 `objValue` 和 `value` 不相等时，就可以将 `value` 设置到 `obj` 上了。

```javascript
const hasOwnProperty = Object.prototype.hasOwnProperty
function assignValue(object, key, value) {
  const objValue = object[key]

  if (!(hasOwnProperty.call(object, key) && eq(objValue, value))) {
    baseAssignValue(object, key, value)
  } else if (value === undefined && !(key in object)) {
    baseAssignValue(object, key, value)
  }
}
```

### 处理正负 `0` 问题

但是 `eq` 是不区分正负 `0` 的，也即`eq(0, -0)` 为 `true`，而 `assignValue` 是要区分正负零的。

如果原来的值为 `0` ，传入的值为 `-0` ，`assignValue` 会将 `0` 更新成 `-0` 。

可以看到，`lodash` 现有的代码是将处理正负零的逻辑放在第一个分支内。

但是假设原来的值 `objValue` 和 传入的值刚好是一对正负零，则 `eq` 得到的结果为 `true`，此时要进入第一个分支的判断，`hasOwnProperty` 要为 `false`，也即 `objValue` 是从原型链上取得的，此时应该直接调用 `baseAssinValue` 更新值即可。

所以在这个分支内的判断我觉得是毫无必要的。

如果 `hasOwnProperty` 为 `true` ，`eq` 也为 `true`，则会跳过第一个分支的逻辑，此时，应该判断一下传入的 `value` 是否等于 `0`(可以为正`0`，也可以为负`0` ，如果为 `0` ，再判断 `objValue` 和 `value` 是否同为正 `0` 或者同为负 `0` ，如果不相同，那要也调用 `baseAssignValue` 来更新值。

源码修改如下：

```javascript
function assignValue(object, key, value) {
  const objValue = object[key]

  if (
    !(hasOwnProperty.call(object, key) && eq(objValue, value)) || 
    (value === 0 && (1 / value !== 1 / objValue)) || 
    (value === undefined && !(key in object))
  ) {
    baseAssignValue(object, key, value)
  }
}
```

至于第三个分支，我觉得是永远不会为 `true` 的，其实可以去掉的。

因为第进入第三个分支，必须要 `hasOwnProperty` 为 `true` ，`eq` 也为 `true` 。

当 `hasOwnProperty` 为 `true` 时，`key in object` 也必定为 `true` ，也即 `!(key in object)` 必定为 `false`，那么 `value === undefined && !(key in object)` 必定为 `false` ，所以这个分支是永远都不会进入的。

以上是我个人的一点见解，也给 `lodash` 提了个 [pr](https://github.com/lodash/lodash/pull/4685)，如果有理解得不正确的地方，还请指正。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 