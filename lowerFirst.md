# lodash源码分析之lowerFirst

本文为读 lodash 源码的第三百二十四篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import createCaseFirst from './.internal/createCaseFirst.js'
```

[《lodash源码分析之createCaseFirst》](./internal/createCaseFirst.md)


## 源码分析

`lowerFirst` 的作用是将第一个字符变成小写。

源码如下：

```javascript
const lowerFirst = createCaseFirst('toLowerCase')
```

其实就是调用 `createCaseFirst` 工厂方法来生成，第一个字符会调用 `toLowerCase` 方法来转换。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

