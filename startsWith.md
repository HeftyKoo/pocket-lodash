# lodash源码分析之startsWith

本文为读 lodash 源码的第三百三十五篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`startsWith` 用来检测字符串 `string` 是否以 `target` 字符串开始，也可以指定开始检测的位置 `position` 。

源码如下：

```javascript
function startsWith(string, target, position) {
  const { length } = string
  position = position == null ? 0 : position
  if (position < 0) {
    position = 0
  }
  else if (position > length) {
    position = length
  }
  target = `${target}`
  return string.slice(position, position + target.length) == target
}
```

和 `endsWith` 类似，只要将开始位置 `position` 到 `postion + target.length` 位置的字符串截取出来，再和 `target` 比较是否相等，即可知道 `string` 是否在指定的位置以 `target` 开始。

### 处理position

```javascript
const { length } = string
position = position == null ? 0 : position
if (position < 0) {
  position = 0
}
else if (position > length) {
  position = length
}
```

如果 `position` 没传，或者传入 `null` ，则默认 `position` 从 `0` 开始。

如果传入的 `position` 小于 `0` ，因为字符串的索引不可能小于 `0` ，则将 `positon` 重置为 `0` 。

如果 `position` 比字符串的长度还要长，超出字符串的长度范围的位置也没有意义，因此重置为 `length` 。

### 比较

```javascript
target = `${target}`
return string.slice(position, position + target.length) == target
```

传入的 `target` 有可能不是字符串，使用模板字符串来转换成字符串。

然后调用字符串的 `slice` 截取 `position` 到 `positon + target.length` 的字符，来和 `target` 比较，如果两者相等，则认为字符串指定的位置是以 `target` 开始的。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

