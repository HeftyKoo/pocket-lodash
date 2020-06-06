# lodash源码分析之range

本文为读 lodash 源码的第三百六十七篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import createRange from './.internal/createRange.js'
```

[lodash源码分析之createRange](./createRange.md)

## 源码分析

`range` 接收 `start` 和 `end` 及 `step` 参数，会创建一个数组，以 `start` 为开始点，下一个数字为上一个数字加上 `step` ，上限不会超过 `end` 。

源码如下：

```javascript
const range = createRange()
```

其实就是调用 `createRange` 来实现，`fromRight` 没有传入。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

