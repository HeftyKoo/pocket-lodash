# lodash源码分析之add

本文为读 lodash 源码的第二百五十五篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import createMathOperation from './.internal/createMathOperation.js'
```

[《lodash源码分析之createMathOperation》](internal/createMathOperation.md)


## 源码分析

`add` 用来将 `augend` 和 `addend` 两个值相加。

源码如下：

```javascript
const add = createMathOperation((augend, addend) => augend + addend, 0)
```

调用 `createMathOperation` 处理规范化两个参数，然后将两者相加。

这里传入的 `defaultValue` 为 `0` 。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

