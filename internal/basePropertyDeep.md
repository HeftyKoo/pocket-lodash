# lodash源码分析之basePropertyDeep

本文为读 lodash 源码的第三百六十二篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseGet from './baseGet.js'
```

[《lodash源码分析之baseGet》](./baseGet.md)


## 源码分析

`basePropertyDeep` 会接收一个属性路径 `path` ，返回一个函数，这个函数接收一个 `object` 参数，这个函数调用时，会取出 `object` 上对应属性路径 `path` 的值。

源码如下：

```javascript
function basePropertyDeep(path) {
  return (object) => baseGet(object, path)
}
```

其实就是调用 `baseGet` 来实现属性路径取值。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

