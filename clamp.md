# lodash源码分析之clamp

本文为读 lodash 源码的第二百七十篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`clamp` 可以传入一个 `number` ，并且可以设定下限 `lower` 和上限 `upper`，如果 `number` 在上限和下限之前，则返回 `number`，如果低于下限，则返回下限值，如果高于上限，则返回上限值。

源码如下：

```javascript
function clamp(number, lower, upper) {
  number = +number
  lower = +lower
  upper = +upper
  lower = lower === lower ? lower : 0
  upper = upper === upper ? upper : 0
  if (number === number) {
    number = number <= upper ? number : upper
    number = number >= lower ? number : lower
  }
  return number
}
```

### 转换成数字

```javascript
number = +number
lower = +lower
upper = +upper
```

先将 `number` 、`lower` 和 `upper` 都转换成数字类型，方便后续比较。

### 处理NaN

```javascript
lower = lower === lower ? lower : 0
upper = upper === upper ? upper : 0
if (number === number) {
  ...
}
return number
```

如果 `lower === lower` 为 `false`，即 `lower` 为 `NaN` 时，将 `lower` 设置为 `0` ，`upper` 的处理同理。

如果 `number === number` 为 `false` ，即 `number` 为 `NaN` 时，因为 `NaN` 无法比较大小，也直接返回 `NaN` 。

### 比较

```javascript
number = number <= upper ? number : upper
number = number >= lower ? number : lower
```

先将 `number` 和上限比较，如果大于上限，则将 `number` 设置为上限。

此时 `number` 相当于上限了，再将 `number` 和下限比较，如果比下限小，将 `number` 设置为下限。

最后是将 `number` 返回。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

