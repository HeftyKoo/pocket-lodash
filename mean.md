# lodash源码分析之mean

本文为读 lodash 源码的第二百六十三篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseMean from './meanBy.js'
```

[《lodash源码分析之meanBy》](meanBy.md)

## 源码分析

`mean` 也是计算一组数的平均值，但是不支持使用迭代器，这就要求传入的数组应全部为数字类型。

源码如下：

```javascript
function mean(array) {
  return baseMean(array, (value) => value)
}
```

可以看到，其实就是调用 `meanBy` ，但是迭代器每次返回的都是原来的数据，不做任何转换。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

