# lodash源码分析之endsWith

本文为读 lodash 源码的第三百一十九篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`endsWith` 判断字符串是否以传入的 `target` 字符串结尾，`endsWith` 支持传入 `position` 参数，表示搜索的结束位置。

源码如下：

```javascript
function endsWith(string, target, position) {
  const { length } = string
  position = position === undefined ? length : +position
  if (position < 0 || position != position) {
    position = 0
  }
  else if (position > length) {
    position = length
  }
  const end = position
  position -= target.length
  return position >= 0 && string.slice(position, end) == target
}
```

### 处理position

```javascript
const { length } = string
position = position === undefined ? length : +position
if (position < 0 || position != position) {
  position = 0
}
else if (position > length) {
  position = length
}
```

如果没有传入 `position`，则 `position` 默认为 `length` ，即字符串最后一位。

如果 `position` 小于 `0` 或者为 `NaN` 则默认为 `0` ，即结束位置为第一位。

如果 `position` 大于 `length` ，即大于字符串的长度，默认结束为最后一位。

### 处理开始位置和结束位置

```javascript
const end = position
position -= target.length
```

结束位置为 `position` ，开始位置是结束位置向前推 `target` 的长度。 

### 得到结果

```javascript
return position >= 0 && string.slice(position, end) == target
```

如果 `position` 小于 `0` ，则 `target` 比要搜索的字符串要长，肯定不会以 `target` 结束。

否则调用 `string` 的 `slice` 方法，根据位置截取要比较的字符串，和 `target` 比较，如果相等，则返回 `true` 。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

