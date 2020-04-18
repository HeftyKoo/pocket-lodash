# lodash源码分析之initCloneObject

本文为读 lodash 源码的第一百九十六篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import isPrototype from './isPrototype.js'
```

[《lodash源码分析之isPrototype》](./isPrototype.md)

## 源码分析

`initCloneObject` 的作用是初始化一个空对象，但是这个空对象会继承传入对象 `object` 的原型链。

源码如下：

```javascript
function initCloneObject(object) {
  return (typeof object.constructor === 'function' && !isPrototype(object))
    ? Object.create(Object.getPrototypeOf(object))
    : {}
}
```

首先判断 `object.constructor` 是否为 `function` ，如果为 `function` 则表示这个对象上可能存在原型链，同时也要将原型对象排除。如果满足这个条件，则使用 `Object.getPrototypeOf` 去获取 `object` 上的原型对象，并用 `Object.create` 创建一个新对象，这个新对象会继承 `object` 的原型链。

否则直接返回一个空对象。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 