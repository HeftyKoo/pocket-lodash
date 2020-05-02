# lodash源码分析之getMatchData

本文为读 lodash 源码的第二百二十六篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import isStrictComparable from './isStrictComparable.js'
import keys from '../keys.js'
```

[《lodash源码分析之isStrictComparable》](./isStrictComparable.md)
[《lodash源码分析之keys》](../keys.md)

## 源码分析

`getMatchData` 是一个内部辅助函数，用来将对象的键值和是否使用 `===` 比较的标记转换成一个数组。

源码如下：

```javascript
function getMatchData(object) {
  const result = keys(object)
  let length = result.length

  while (length--) {
    const key = result[length]
    const value = object[key]
    result[length] = [key, value, isStrictComparable(value)]
  }
  return result
}
```

先用 `keys` 取出对象 `object` 自身可枚举的非 `Symbol` 类型的属性。

然后遍历属性，取出 `object` 对应属性 `key` 的值，并且计算 `value` 是否可以使用 `===` 比较的标记，组成 `[key, value, flag]` 形式的数组，存入 `result` 中。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 