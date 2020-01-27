# lodash源码分析之pull

本文为读 lodash 源码的第六十二篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import pullAll from './pullAll.js'
```

[《lodash源码分析之PullAll》](pullAll.md)

## 源码分析

```javascript
function pull(array, ...values) {
  return pullAll(array, values)
}
```

可以看到，`pull` 其实调用的是 `pullAll`。`pullAll` 第二个参数接收的是数组，`pull` 接收的是不定参数，这里用 `...` 来将后面的参数组成数组传给 `pullAll`。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 