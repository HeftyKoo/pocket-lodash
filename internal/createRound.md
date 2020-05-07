# lodash源码分析之createRound

本文为读 lodash 源码的第二百五十六篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)


## 源码分析

`createRound` 用来创建类似于 `Math.round` 、`Math.ceil` 之类的舍入取整方法。

`Math.round` 之类的原生方法，经过转换后会是一个整数，但是 `createRound` 可以根据精度取整。

例如使用 `createRound` 创建一个 `ceil` 方法：

```javascript
const ceil = createRound('ceil')
ceil(6.004, 2) // => 6.01
ceil(6040, -2) // => 6100
```

`6.004`，因为指定精度是 `2` ，而且是向上取整，因此得到的是 `6.01` 。

`6040` 因为指定的精度为 `-2` ，因此从小数点往前数（这里没有小数点，或者说小数点在最后一位）两位，即到 `60` 处，下一位是 `4` ，向上取整得到 `6100` 。

源码如下：

```javascript
function createRound(methodName) {
  const func = Math[methodName]
  return (number, precision) => {
    precision = precision == null ? 0 : (precision >= 0 ? Math.min(precision, 292) : Math.max(precision, -292))
    if (precision) {
      let pair = `${number}e`.split('e')
      const value = func(`${pair[0]}e${+pair[1] + precision}`)

      pair = `${value}e`.split('e')
      return +`${pair[0]}e${+pair[1] - precision}`
    }
    return func(number)
  }
}
```

`methodName` 是指要创建什么样的方法，对应 `Math` 中的 `round` 、`ceil` 和 `floor` 等。

先用 `Math[methodName]` 将 `Math` 中对应的方法 `func` 取出。

可以看到，调用 `createRound` 最终会返回一个方法，可以将 `createRound` 理解为一个工厂方法。

### 计算精度

```javascript
precision = precision == null ? 0 : (precision >= 0 ? Math.min(precision, 292) : Math.max(precision, -292))
```

如果没有传入精度，则精度为 `0` ，这跟直接调用 `Math` 对应方法的效果一样。

如果 `percision` 为正数，则取 `292` 和 `precision` 的较小者，因为后面会对 `number` 做放大操作，如果最大值 `Math.MAX_VALUE` 和 `1e292` 相加，会得到 `INFINITY`。

同理，如果 `precision` 小于 `0` ，则取 `-292` 和 `precison` 中的较大者。

### 精度为0的情况

在精度为 `0` 的情况下，直接调用 `func[number]` ，没有做任何转换，其实和直接调用 `Math` 上对应的方法没有任何差别。

### 处理精度

#### 缩放

```javascript
let pair = `${number}e`.split('e')
const value = func(`${pair[0]}e${+pair[1] + precision}`)
```

因为 `Math` 的函数只能处理整数，因此要精确到小数位和支持负精度，需要对 `number` 进行缩放。

缩放本来很简单，只需要这样即可：

```javascript
number * 10 * precision
```

但是如果是浮点数，在进行乘法运算时，可能会出现精度丢失的情况。

因此这里使用了一种比较迂回的方式，先将数字转换成字符串，使用字符串科学记数法拼接的方式来进行缩放，再传给 `func` 转换。

`pair` 得到的是类似这样的数组 `[有效数, 位数, '']` ，因此只需要对位数进行缩放即可。

也即使用科学记数法，放大后的科学记数法字符串是 `${pair[0]}e${+pair[1] + precision} `。

在这里就可以调用 `func` 得到缩放取整后的结果。

### 反向缩放

```javascript
pair = `${value}e`.split('e')
return +`${pair[0]}e${+pair[1] - precision}`
```

因为在取整的时候是要进行缩放的，在取整得到结果后，需要反向缩放，才能得到真正的结果。

反向缩放的原理和上面的一样。

## 参考资料

https://www.cnblogs.com/xiaohuochai/p/5586166.html

https://www.cnblogs.com/zhoulujun/p/10880912.html

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

