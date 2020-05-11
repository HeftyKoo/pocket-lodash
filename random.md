# lodash源码分析之random

本文为读 lodash 源码的第二百七十三篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import toFinite from './toFinite.js'
```

[《lodash源码分析之toFinite》](toFinite.md)

## 源码分析

`random` 用来得到一个随机数，和 `Math.random` 不同的是，`random` 可以指定下限 `lower` 和上限 `upper` ，如果有指定，会返回 `lower` 和 `upper` 之间的随机数。如果不指定，则返回 `0` 到 `1` 之间的随机数，如果只指定一个，则这个数会被当作为上限 `upper` 。

默认情况下，`random` 返回的是整数随机数，可以指定 `floating` 来得到浮点数的随机数，如果传入的上限或者下限如果有浮点数，也会得到浮点数的随机数。

源码如下：

```javascript
const freeParseFloat = parseFloat
function random(lower, upper, floating) {
  if (floating === undefined) {
    if (typeof upper === 'boolean') {
      floating = upper
      upper = undefined
    }
    else if (typeof lower === 'boolean') {
      floating = lower
      lower = undefined
    }
  }
  if (lower === undefined && upper === undefined) {
    lower = 0
    upper = 1
  }
  else {
    lower = toFinite(lower)
    if (upper === undefined) {
      upper = lower
      lower = 0
    } else {
      upper = toFinite(upper)
    }
  }
  if (lower > upper) {
    const temp = lower
    lower = upper
    upper = temp
  }
  if (floating || lower % 1 || upper % 1) {
    const rand = Math.random()
    const randLength = `${rand}`.length - 1
    return Math.min(lower + (rand * (upper - lower + freeParseFloat(`1e-${randLength}`))), upper)
  }
  return lower + Math.floor(Math.random() * (upper - lower + 1))
}

```

### 处理参数

#### 处理floating 没有传入的情况

`floating` 是一个布尔值，作为第三个参数传入，如果第三个参数没有传入，则需要检测 `upper` 和 `lower` 是否传入了布尔值。

相关代码：

```javascript
if (floating === undefined) {
  if (typeof upper === 'boolean') {
    floating = upper
    upper = undefined
  }
  else if (typeof lower === 'boolean') {
    floating = lower
    lower = undefined
  }
}
```

如果 `upper` 传入了布尔值，则表示 `upper` 这个位置传入的是 `floating` ，因此将 `upper` 的值赋给 `floating` ，并且将 `upper` 置为 `undefined` ，相当于 `upper` 没有传入。

如果 `upper` 位置没有传入布尔值，再检测 `lower` 位置有没有传入布尔值，处理逻辑和 `upper` 相同。

#### 上限和下限都没有传入

```javascript
if (lower === undefined && upper === undefined) {
  lower = 0
  upper = 1
}
```

可以看到，上限和下限都没有传入时，默认下限 `lower` 为 `0` ，上限 `upper` 为 `1` 。

#### 处理上限

如果没有出现上限和下限都没有传的情况，则还可能会出现只传了下限而没有传上限的情况：

```javascript
if (lower === undefined && upper === undefined) {
  ...
}
else {
  lower = toFinite(lower)
  if (upper === undefined) {
    upper = lower
    lower = 0
  } else {
    upper = toFinite(upper)
  }
}
```

在这种情况下，下限肯定是有传的，但是可能会中无限值，因此先调用 `toFinite` 来确保是有限值。

然后检测上限 `upper` 是否有传，如果没有传，则将 `lower` 当作上限 `upper` ，将 `lower` 默认为 `0` 。

如果有传，则也调用 `toFimite` 来确保 `upper` 也为有限值。

至此，下限 `lower` 和 `upper` 都已得到一个有限值。

#### 上限和下限的大小比较

现在虽然下限和上限都已经有值，但是可能会出现下限比上限值要大的情况：

```javascript
if (lower > upper) {
  const temp = lower
  lower = upper
  upper = temp
}
```

如果出现下限比上限值大的情况，则将两者交换，有确保下限值比上限值小。

### 浮点随机数生成

```javascript
if (floating || lower % 1 || upper % 1) {
  const rand = Math.random()
  const randLength = `${rand}`.length - 1
  return Math.min(lower + (rand * (upper - lower + freeParseFloat(`1e-${randLength}`))), upper)
}
```

可以看到，只要设置了 `floating` 或者 `lower` 和 `upper` 两者中只要出现浮点数，最后得到的随机数也为浮点数。

要得到 `lower` 和 `upper` 之间的浮点随机数，不是像以下这样就可以了吗？

```javascript
const rand = Math.random()
return lower + rand * (upper - lower)
```

这个看起来很简单的问题，为什么 `lodash` 要搞得这么复杂呢？

原来 `lodash` 的 `random` 方法返回的 `lower` 和 `upper` 之间的随机数，这随机数其实是包括下限和上限的，但是 `Math.random` 返回的是 `0` 到 `1` 之间的随机数，但是不包含 `1` 的，即不包括上限。

因此 `lodash` 费尽心思要让最后产生的随机数能有返回上限，但是返回上限的机率又不能过大。

`lodash` 想到的是，`rand * (upper - lower)` 这里改一下，因为 `rand` 总是小于 `1` 的，那只要 `upper - lower` 加上一个极小值，则 `rand * (upper - lower)` 就有可能大于 `upper - lower` ， `lodash` 使用科学计数法来构造这个极小值。

最后使用 `Math.min` 来返回随机数和 `upper` 之间的较小者，因为随机数是有可能比 `upper` 大的。

### 整数随机数的生成

```javascript
return lower + Math.floor(Math.random() * (upper - lower + 1))
```

相对于浮点数来说，整数就简单多了，因为 `Math.random` 返回的随机值不包含 `1` ，只需要将 `upper - lower` 的差加上 `1` 即可得到可能比 `upper` 大的随机值，并且这个随机值又小于 `upper + 1` 。

因为要生成的是整数随机数，因此需要调用 `Math.floor` 向下取整，将小数部分抹除。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

