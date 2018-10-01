# lodash源码分析之last

> 我把我整个灵魂都给你，连同它的怪癖，耍小脾气，忽明忽暗，一千八百种坏毛病。它真讨厌，只有一点好，爱你。
>
> ——王小波

本文为读 lodash 源码的第二十八篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`last` 函数用来获取数组的最后一项，源码如下：

```javascript
function last(array) {
    const length = array == null ? 0 : array.length
    return length ? array[length - 1] : undefined
}
```

如果有传递数组，则用 `array.length` 获取数组的长度，没有传递数组，则 `length` 赋值为 `0` 。

如果数组长度存在，则返回数组最后一项，否则返回 `undefined` 。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

