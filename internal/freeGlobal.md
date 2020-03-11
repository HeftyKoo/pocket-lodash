# lodash源码分析之freeGlobal

本文为读 lodash 源码的第一百二十四篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`freeGlobal` 是用来在 `node` 环境中捕获 `global` 变量，这个 `global` 变量有点相当于浏览器环境中的 `window` 对象。

```javascript
const freeGlobal = typeof global === 'object' && global !== null && global.Object === Object && global

export default freeGlobal
```

首先 `global` 必须是一个 `object` ，而且不能为 `null` 。

接着判断 `global.Object` 是否和全局的 `object` 构造函数相同，如果相同，则这个很大概率就是全局的 `global` 对象了。

如果前面的判断都为 `true`，则将 `global` 对象返回。

但是这个检测也是有缺陷的，因为 `global` 并不是 `js` 中的保留词，因为如果在浏览器中这样定义：

```javascript
var global = {
  Object: Object
}
```

也是可以冒充 `global` 对象的。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 