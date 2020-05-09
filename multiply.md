# lodash源码分析之multiply

本文为读 lodash 源码的第二百六十五篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import createMathOperation from './.internal/createMathOperation.js'
```

[lodash源码分析之createMathOperation](internal/createMathOperation.md)

## 源码分析

`multiply` 的作用是计算两个数的积。

源码如下：

```javascript
const multiply = createMathOperation((multiplier, multiplicand) => multiplier * multiplicand, 1)
```

调用 `createMathOperation` 来处理参数，默认值为 `1` 。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

