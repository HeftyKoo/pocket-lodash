# lodash源码分析之zip

本文为读 lodash 源码的第一百一十四篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import unzip from './unzip.js'
```
[《lodash源码分析之unzip》](unzip.md)

## 源码分析

`zip` 方法和 `unzip` 类似，但是 `zip` 接入的不是一组数组，而是不定参数，每个参数都是数组。

源码如下：

```javascript
function zip(...arrays) {
  return unzip(arrays)
}
```

可以看到 `zip` 使用 `...` 操作符将所有参数收集在 `arrays` 中，然后直接调用 `unzip` 来实现。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 