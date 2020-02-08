# lodash源码分析之castPath

本文为读 lodash 源码的第六十九篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import isKey from './isKey.js'
import stringToPath from './stringToPath.js'
```

[《lodash源码分析之isKey》](./isKey.md)

[《lodash源码分析之stringToPath》](./stringToPath.md)

## 源码分析

`castPath` 是 `lodash` 的内部方法，其实是在 `stringToPath` 上再封装一层，相对来说，`stringToPath` 更加纯粹，`castPath` 则会在上层做一些校验。

源码如下：

```javascript
function castPath(value, object) {
  if (Array.isArray(value)) {
    return value
  }
  return isKey(value, object) ? [value] : stringToPath(value)
}
```

因为最后返回的是数组，所以最开始会先检测是不是数组，如果是数组，则直接将值返回。

例如在 `get` 函数中，第二个参数其实不一定是字符串的，也可以是传入一个路径数组。

接下来，如果 `value` 本身就是属性，那也没必要再使用 `stringToPath` 转换成路径数组，因为如果 `value` 是属性的话，那路径数组只会有一个值，返回 `[value]` 即可。

否则调用 `stringToPath` 去解析。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 