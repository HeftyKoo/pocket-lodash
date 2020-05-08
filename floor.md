# lodash源码分析之floor

本文为读 lodash 源码的第二百五十九篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import createRound from './.internal/createRound.js'
```

[lodash源码分析之createRound](internal/createRound.md)

## 源码分析

`floor` 的作用和 `Math.floor` 类似，不过 `floor` 可以指定精度。

源码如下：

```javascript
const floor = createRound('floor')
```

其实就是使用工厂方法 `createRound` 所创建的函数。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

