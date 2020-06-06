# lodash源码分析之createRange

本文为读 lodash 源码的第三百六十六篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseRange from './baseRange.js'
import toFinite from '../toFinite.js'
```

[lodash源码分析之baseRange](./baseRnage.md)

[lodash源码分析之toFinite](./toFinite.md)

## 源码分析

`createRange` 用来创建一个生成范围数组的函数，接收一个参数 `fromRight` ，如果 `fromRight` 为 `true` ，所创建的函数生成的范围数组从大到小排序。

源码如下：

```javascript
function createRange(fromRight) {
  return (start, end, step) => {
    start = toFinite(start)
    if (end === undefined) {
      end = start
      start = 0
    } else {
      end = toFinite(end)
    }
    step = step === undefined ? (start < end ? 1 : -1) : toFinite(step)
    return baseRange(start, end, step, fromRight)
  }
}
```

其实返回的函数，最终会调用 `baseRange` 来实现，但是在调用之前，会对参数进行合规化处理。

### 处理start和end

先是调用 `toFinite` 来确保 `start` 为有限数字。

如果 `end` 为 `undefined` ，则将 `start` 赋值给 `end` ，并将 `start` 设置为 `0` ，这种情况下生成的数组中的数值范围在 `0 - start` 之间。

如果有传 `end` ，也调用 `toFinite` 将 `end` 转换成有限数字。

### 处理step

如果 `step` 没传，则判断 `start` 是否比 `end` 小，如果是，则 `step` 默认为 `1` ，否则为 `-1` ，因为 `start` 比 `end` 大时，需要的是递减数组。

如果有传，则调用 `toFinite` 确保 `step` 为有限数字。

最终调用 `baseRange` 来创建数组。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

