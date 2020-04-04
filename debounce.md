# lodash源码分析之debounce

本文为读 lodash 源码的第一百七十三篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import isObject from './isObject.js'
import root from './.internal/root.js'
```

[《lodash源码分析之isObject》](isObject.md)
[《lodash源码分析之root》](internal/root.md)

## 源码分析

`debounce` 就是我们通常所说的防抖，防抖的作用是防止函数在一段时间内过快而又无效的执行。

防抖的原理是在事件触发时，会记录当前事件触发的时间，如果在允许等待的时间 `wait` 内又触发了事件，则函数还不会执行，但是以这个时间为准，再等 `wait` 的时间，如果再无事件触发，函数才会执行。

`debounce` 最经典的应用场景是用在搜索建议上，搜索建议会在你输入结束或者输入时间间隔比较长时，才会请求获取建议列表。

### 最简单实现

知道原理后，可以很简单地实现一个 `debounce` 函数：

```javascript
function debounce (func, wait) {
  let timerId
  function debounced () {
    clearTimeout(timerId)
    timerId = setTimeout(func, wait)
  }
}
```

这样就实现了最简单的 `debounce` 函数了。

但是用户在使用的时候，可能会不按照要求传入参数，为了更加严谨和友好，再对参数进行一些处理。

我们希望传入的 `func` 为一个函数，如果不是函数则报错；希望 `wait` 是一个数字，如果不是数字，则转换成数字类型。

代码修改如下：

```javascript
function debounce (func, wait) {
  let timerId
  
  if (typeof func !== 'function') {
    throw new TypeError('Expected a function')
  }
  
  wait = +wait || 0
  function debounced () {
    clearTimeout(timerId)
    timerId = setTimeout(func, wait)
  }
}
```

`wait` 转换成数字后，也有可能是 `NaN` 的情况，这里默认为 `0`。

### 使用 `requestAnimationFrame` 优化

如果 `wait` 没有传递，按照之前的实现，是使用 `setTimeout` ，时间设置为 `0` 来实现的。

现代浏览器有一个 `requestAnimationFrame` 的 `api` ，会在浏览器重绘之前调用，性能比 `setTimeout` 更好。

因此可以这样判断是否需要使用 `requestAnimationFrame`：

```javascript
const useRAF = (!wait && wait !== 0 && typeof root.requestAnimationFrame === 'function')
```

只有在 `wait` 为假值， 并且 `wait` 也没有指定为 `0` ，并且当前环境支持 `requestAnimationFrame` 的情况下，才使用 `requestAnimationFrame` 。

代码更改如下：

```javascript
function debounce (func, wait) {
  let timerId
  const useRAF = (!wait && wait !== 0 && typeof root.requestAnimationFrame === 'function')
  
  if (typeof func !== 'function') {
    throw new TypeError('Expected a function')
  }
  
  wait = +wait || 0
  function debounced () {
    if (useRAF) {
      return root.cancelAnimationFrame(timerId)
    } else {
      clearTimeout(timerId)
    }
    
    if (useRAF) {
      root.cancelAnimationFrame(timerId)
      timerId = root.requestAnimationFrame(func)
    } else {
      timerId = setTimeout(func, wait)
    }
  }
}
```

现在代码有点复杂了，可以将开启计数器，和清除计数器的逻辑抽象成两个函数：

```javascript
function debounce (func, wait) {
  let timerId
  const useRAF = (!wait && wait !== 0 && typeof root.requestAnimationFrame === 'function')
  
  if (typeof func !== 'function') {
    throw new TypeError('Expected a function')
  }
  
  wait = +wait || 0
  
  function startTimer (pendingFunc, wait) {
    if (useRAF) {
      root.cancelAnimationFrame(timerId)
      return root.requestAnimationFrame(pendingFunc)
    }
    return setTimeout(pendingFunc, wait)
  }
  
  function clearTimer (id) {
    if (useRAF) {
      return root.cancelAnimationFrame(id)
    }
    clearTimeout(id)
  }
  
  function debounced () {
    clearTimer(timerId)
    startTimer(func, wait)
  }
}
```

### 参数、`this` 及返回值

目前的实现，在最后调用 `func` 的时候是没有传入参数的，而且函数调用的时候的 `this` 也没有绑定，可能跟预期的指向不一致，也没有返回值。

因为最后返回调用的函数是 `debounced`，因此可以将 `debounced` 修改如下：

```javascript
function debounced (...args) {
  let lastThis = this
  let result
  clearTimer(timerId)
  
  startTimer(function () {
    result = func.apply(lastThis, args)
  }, wait)
  
  startTimer(func, wait)
  
  return result
}
```

因为 `func` 是延后调用的，`lodash` 在对返回值的处理是返回上一次调用 `func` 的结果。

同样，我们也可以对这部分逻辑进行抽离。

```javascript
function debounce (func, wait) {
  let lastArgs,
      lastThis,
      result,
      timerId
  
  const useRAF = (!wait && wait !== 0 && typeof root.requestAnimationFrame === 'function')
  
  if (typeof func !== 'function') {
    throw new TypeError('Expected a function')
  }
  
  wait = +wait || 0
  
  function invokeFunc () {
    const args = lastArgs
    const thisArg = lastThis
    
    lastArgs = lastThis = undefined
    result = func.apply(thisArg, args)
   	return result
  }
  
  function startTimer (pendingFunc, wait) {
    if (useRAF) {
      root.cancelAnimationFrame(timerId)
      return root.requestAnimationFrame(pendingFunc)
    }
    return setTimeout(pendingFunc, wait)
  }
  
  function clearTimer (id) {
    if (useRAF) {
      return root.cancelAnimationFrame(id)
    }
    clearTimeout(id)
  }
  
  function debounced (...args) {
    lastArgs = args
    lastThis = this
    clearTimer(timerId)
    startTimer(invokeFunc, wait)
  }
}
```

提取出来的 `invokeFunc` 会对 `lastArgs` 和 `lastThis` 做一些重置的工作。

### 最大等待时间

现在 `debounced` 的设计是，如果事件触发的间隔总不超过 `wait` ，则会一直不会调用到 `func` ，但是有些场景下，我们希望可以设置一个最大的等待时间 `maxWait` ，如果上一次调用 `func` 的时间间隔超过 `maxWait` ，则无论事件的触发间隔是否超过 `wait` ，都会调用 `func` 。

考虑到这个是可选的配置，我们为 `debounce` 增加第三个参数 `options` ，`options` 为对象，方便后续的扩展。

增加 `maxWait` 后，我们需要对 `maxWait` 进行取值：

```javascript
let maxWait
let maxing = false

if (isObject(options)) {
  maxing = 'maxWait' in options
  maxWait = maxing ? Math.max(+options.maxWait || 0, wait) : maxWait
}
```

用 `maxing` 来表示有没有传入 `maxWait` 参数，也即表示要不要开启最大等待时间这个功能。

用户还可能传入 `maxWait` 的值比 `wait` 还要小，因此要取 `maxWait` 和 `wait` 之间的较大值来作为最大等待时间。

```javascript
let lastCallTime
let lastInvokeTime = 0
```

因为涉及到了两个时间的竞争，这里用 `lastCallTime` 来记录最后一次 `debounced` 调用时的时间，用 `lastInvokeTime` 来记录最后一次 `func` 被调用时的时间。

`lastInvokeTime` 在 `invokeFunc` 里保存：

```javascript
function invokeFunc(time) {
  const args = lastArgs
  const thisArg = lastThis

  lastArgs = lastThis = undefined
  lastInvokeTime = time
  result = func.apply(thisArg, args)
  return result
}
```

`lastCallTime` 在 `debounced` 调用时保存：

```javascript
function debounced (...args) {
  const time = Date.now()
  
  lastArgs = args
  lastThis = this
  lastCallTime = time
  
  clearTimer(timerId)
  startTimer(invokeFunc, wait)
}
```

因为涉及到了 `wait` 和 `maxWait` 的竞争，这时，我们就不能每次调用 `debounced` 的时候都直接 `clearTimer` 了和 `startTimer` 了，因为这只是以 `wait` 为维度的。

因为 `debounced` 会多次调用，又不能每次都重置 `timer` ，因此需要计算一个状态来是否开启 `timer` 。

以下为源码：

```javascript
function shouldInvoke(time) {
  const timeSinceLastCall = time - lastCallTime
  const timeSinceLastInvoke = time - lastInvokeTime

  return (lastCallTime === undefined || (timeSinceLastCall >= wait) ||
          (timeSinceLastCall < 0) || (maxing && timeSinceLastInvoke >= maxWait))
}
```

逐个条件分析下：

`lastCallTime === undefined` ，即第一次调用的时候

`timeSinceLastCall >= wait`，即事件触发的间隔超过 `wait` 时，这是 `debounce` 最开始就要支持的功能。

`timeSiceLastCall < 0` ，这个从逻辑上很好理解，但是从场景上有点难理解，怎么会在下次检测的时候，`time` 会比 `lastCallTime` 还要小的情况呢？难道有时光机？其实这种应该算是边缘情况，例如在修改系统时间的情况下。

`maxing && timeSinceLastInvoke >= maxWait`， 最后这个条件是处理最大等待时候的情况，如果 `timeSinceLastInvoke` 比最大的等待时间 `maxWait` 还要大，也要重新开启 `timer` 。

有了判断条件，还要计算 `timer` 的时间间隔，之前直接使用 `wait` ，现在因为有了 `maxWait` ，因此时间间隔也要使用这两者进行计算了。

源码如下：

```javascript
function remainingWait(time) {
  const timeSinceLastCall = time - lastCallTime
  const timeSinceLastInvoke = time - lastInvokeTime
  const timeWaiting = wait - timeSinceLastCall

  return maxing
    ? Math.min(timeWaiting, maxWait - timeSinceLastInvoke)
  : timeWaiting
}
```

这个看其实很易理解，使用 `wait - timeSinceLastCall` 可以计算出没有 `maxWait` 的时候的等待时间。

如果要支持最大的等待时候，可以使用 `maxWait - timeSinceLastInvoke` 得出最大的等待时间所剩余的时间。

然后取这两者中较小者就可以得出 `timer` 所要求的时间间隔了。

因为 `wait` 肯定不会大于 `maxWait`，因此我们在第一次进入的时候调用 `startTimr` 时传入的时间间隔应为 `wait` ，但是这时传给 `timer` 的回调函数不能直接是 `func` ，因为这样会忽略掉 `maxWait` 的情况。

在这里，我们使用一个 `timerExpired` 函数来进行 `timer` 的重启工作。

源码如下：
```javascript
function timerExpired() {
  const time = Date.now()
  if (shouldInvoke(time)) {
    timerId = undefined
    return invokeFunc(time)
  }
  // Restart the timer.
  timerId = startTimer(timerExpired, remainingWait(time))
}
```

如果在 `wait` 时间过后，`shouldInvkoe` 为 `false` 则表示还没达到调用 `func` 的条件，这里需要考虑 `maxWait` 的情况，因此再次调用 `startTimer` ，这次传入的时间为 `remainingWait(time)` ，即重新计算后的等待时间来重启 `timer` 。

如果达到调用 `func` 的条件，直接调用 `func` 即可。

`debounced` 函数也需要作如下的修改：

```javascript
function debounced (...args) {
  const time = Date.now()
  const isInvoking = shouldInvoke(time)

  lastArgs = args
  lastThis = this
  lastCallTime = time
  
  if (isInvoking) {
    if (timeId === undefined) {
      lastInvokeTime = lastCallTime
      return startTimer(timerExpired, wait)
    }
    
    if (maxing) {
      timerId = startTimer(timerExpired, wait)
      return invokeFunc(lastCallTime)
    }
  }
  
  return result
}
```

在调用 `debounced` 函数时，如果 `isInvoking` 为 `true` ，我们先忽略掉 `if(maxing)` 这个分支，理解起来就很简单了，如果 `isInvoking` 为 `true` ，并且当前 `timerId` 为 `undefined` ，表示需要启动 `timer` 了，直接调用 `startTimer` 即可。

`if(maxing)` 这个分支一开始我也不太理解，后来看到这篇文章[《探究防抖(debounce)和节流(throttle)》](https://github.com/Bowen7/Blog/issues/5)和看了对应的测试用例之后，才知道原因。

这篇文章写得挺详细，摘录如下：

> 一开始我没看明白 if(maxing)里面这段代码的作用，按理说，是不会执行这段代码的，后来我去 lodash 的仓库里看了 test 文件，发现对这段代码，专门有一个 case 对其测试。我剥除了一些代码，并修改了测试用例以便展示，如下：
>
> ```
> var limit = 320,
>   withCount = 0;
> 
> var withMaxWait = debounce(
>   function() {
>     console.log("invoke");
>     withCount++;
>   },
>   64,
>   {
>     maxWait: 128
>   }
> );
> 
> var start = +new Date();
> while (new Date() - start < limit) {
>   withMaxWait();
> }
> ```
>
> 执行代码，打印了 3 次 invoke；我又将 if(maxing){}这段代码注释，再执行代码，结果只打印了 1 次。结合源码的英文注释`Handle invocations in a tight loop`，我们不难理解，原本理想的执行顺序是 withMaxWait->timer->withMaxWait->timer 这种交替进行，但由于 setTimeout 需等待主线程的代码执行完毕，所以这种短时间快速调用就会导致 withMaxWait->withMaxWait->timer->timer，从第二个 timer 开始，由于 lastArgs 被置为 undefined，也就不会再调用 invokeFunc 函数，所以只会打印一次 invoke。
>
> 同时，由于每次执行 invokeFunc 时都会将`lastArgs`置为 undefined，在执行 trailingEdge 时会对 lastArgs 进行判断，确保不会出现执行了 if(maxing){}中的 invokeFunc 函数又执行了 timer 的 invokeFunc 函数
>
> 这段代码保证了设置 maxWait 参数后的正确性和时效性

汇总一下代码：

```javascript
function debounce(func, wait, options) {
  let lastArgs,
    lastThis,
    maxWait,
    result,
    timerId,
    lastCallTime

  let lastInvokeTime = 0
  let maxing = false

  const useRAF = (!wait && wait !== 0 && typeof root.requestAnimationFrame === 'function')

  if (typeof func !== 'function') {
    throw new TypeError('Expected a function')
  }
  wait = +wait || 0
  if (isObject(options)) {
    maxing = 'maxWait' in options
    maxWait = maxing ? Math.max(+options.maxWait || 0, wait) : maxWait
  }

  function invokeFunc(time) {
    const args = lastArgs
    const thisArg = lastThis

    lastArgs = lastThis = undefined
    lastInvokeTime = time
    result = func.apply(thisArg, args)
    return result
  }

  function startTimer(pendingFunc, wait) {
    if (useRAF) {
      root.cancelAnimationFrame(timerId)
      return root.requestAnimationFrame(pendingFunc)
    }
    return setTimeout(pendingFunc, wait)
  }

  function cancelTimer(id) {
    if (useRAF) {
      return root.cancelAnimationFrame(id)
    }
    clearTimeout(id)
  }

  function remainingWait(time) {
    const timeSinceLastCall = time - lastCallTime
    const timeSinceLastInvoke = time - lastInvokeTime
    const timeWaiting = wait - timeSinceLastCall

    return maxing
      ? Math.min(timeWaiting, maxWait - timeSinceLastInvoke)
      : timeWaiting
  }

  function shouldInvoke(time) {
    const timeSinceLastCall = time - lastCallTime
    const timeSinceLastInvoke = time - lastInvokeTime

    return (lastCallTime === undefined || (timeSinceLastCall >= wait) ||
      (timeSinceLastCall < 0) || (maxing && timeSinceLastInvoke >= maxWait))
  }

  function timerExpired() {
    const time = Date.now()
    if (shouldInvoke(time)) {
      timerId = undefined
      return invokeFunc(time)
    }
    timerId = startTimer(timerExpired, remainingWait(time))
  }

 
  function debounced(...args) {
    const time = Date.now()
    const isInvoking = shouldInvoke(time)

    lastArgs = args
    lastThis = this
    lastCallTime = time

    if (isInvoking) {
      if (timerId === undefined) {
        lastInvokeTime = lastCallTime
        return startTimer(timerExpired, wait)
      }
      if (maxing) {
        // Handle invocations in a tight loop.
        timerId = startTimer(timerExpired, wait)
        return invokeFunc(lastCallTime)
      }
    }
   
    return result
  }
  return debounced
}

```

### 启用定时器前先调用 `func`

按照现在的实现方式，会在两个事件的触发时间大于 `wait` 或者等待的时间大于 `maxWait` 时才会调用 `func` 。

但是如果没有设置 `maxWait`，而事件的触发时间间隔一直小于 `wait` ，就可能会导致 `func` 在很长的时间内得不到触发，这时会希望有一个开关，可以控制在设置 `timer` 前先调用 `func` ，这样 `func` 至少有一次调用的机会。

这个开关同样也放在 `options` 中，用 `leading` 来表示。

获取值：

```javascript
let leading = false

if (isObject(options)) {
  leading = !!options.leading
}
```

我们假设有一个函数 `leadingEdge` 来判断是否需要调用 `func` ，先不管这个函数的具体实现，则 `debounced` 函数需要修改如下：

```javascript
function debounced(...args) {
  const time = Date.now()
  const isInvoking = shouldInvoke(time)

  lastArgs = args
  lastThis = this
  lastCallTime = time

  if (isInvoking) {
    if (timerId === undefined) {
      return leadingEdge(lastCallTime)
    }
    if (maxing) {
      // Handle invocations in a tight loop.
      timerId = startTimer(timerExpired, wait)
      return invokeFunc(lastCallTime)
    }
  }

  return result
}
```

这很易理解，因为 `timerId` 为 `undefined`，又处于需要启动 `timer` 的状态，所以需要调用 `leadingEdge` 。

现在来看看 `leadingEdge` 函数的实现：

```javascript
function leadingEdge(time) {
  lastInvokeTime = time
  timerId = startTimer(timerExpired, wait)
  return leading ? invokeFunc(time) : result
}
```

这里前两行是之前的逻辑，第三行就个判断，如果 `leading` 为 `true` ，则马上调用 `invokeFunc` ，不需要等待 `timer` 跑完。

### 定时器跑完后不调用 `func`

定时器跑完前可以用 `leading` 来控制是否调用 `func` ，但是有时也想定时器跑完后不调用 `func` ，目前的实现是定时器跑完后都调用 `func` 的。

同样在 `options` 中增加一个配置 `trailing`。

```javascript
let trailing = true

if (isObject(options)) {
  trailing = 'trailing' in options ? !!options.trailing : trailing
}
```

然后从 `trailing` 中获取值。

我们假设控制定时器跑完后是否调用 `func` 的逻辑在 `trailingEdge` 中，则 `timerExpired` 需要作如下修改：

```javascript
function timerExpired() {
  const time = Date.now()
  if (shouldInvoke(time)) {
    return trailingEdge(time)
  }
  
  timerId = startTimer(timerExpired, remainingWait(time))
}
```

`trailingEdge` 的源码如下：

```javascript
function trailingEdge(time) {
  timerId = undefined

  if (trailing && lastArgs) {
    return invokeFunc(time)
  }
  lastArgs = lastThis = undefined
  return result
}
```

原来 `invokeFunc` 里会重置 `lastArgs` 和 `lastThis` ，但是因为有 `trailing` 这个开关，`invokeFunc` 可能会调用不到，因此在这里也将 `lastArgs` 和 `lastThis` 重置。

这里除了 `trailing` 的判断外还有 `lastArgs` 的判断，我们先不管 `lastArgs` 在那里设置为 `undefined`，从代码直观上来看，没有 `lastArgs` 调用 `invokeFunc` 可能会出错，因为 `func` 可能会得不到它想要的参数。

其实后面会看到，手动调用 `flush` 函数的时候，可能会将 `lastArgs` 设置为 `undefined` ，如果 `timer` 时间到的时候，这里 `invokeFunc` 是不应该执行的，因此还没有获得它所需要的参数。

### 其他方法

其实 `debounce`  到这里已经实现完毕了，但是 `debounce` 还提供一些方法来供外界控制 `debounce` 的内部进程，和查询进度，这些方法作为 `debounce` 返回的函数 `debounced` 的属性来提供给外界使用。

#### cancel

`cancel` 方法其实在一开始的时候就已经实现，后来就不需要用到了，但是提供给外界可以取消 `timer` 也是挺有用的：

```javascript
function cancelTimer(id) {
  if (useRAF) {
    return root.cancelAnimationFrame(id)
  }
  clearTimeout(id)
}
debounced.cancel = cancel
```

#### flush

`flush` 可以控制 `func` 是否立即执行，不需要等待 `timer` 时间到后再触发，源码如下：

```javascript
function flush() {
  return timerId === undefined ? result : trailingEdge(Date.now())
}
debounced.flush = flush
```

如果没有 `timerId` 表示 `func` 已经执行过，或者第一次还没有调用，这里是没有 `lastArgs` 的，直接返回上一次的结果 `result` 即可。

否则调用 `traillingEdge` 方法去调用 `func` ，得到结果。

因为手动调用了 `traillingEdge` ，会将 `timerId` 设置为 `undefined` ，这时如果再调用 `debounced` 时， `shouldInvoke` 可能返回的是 `false` ，但是因为已经调用过 `flush` ，需要重新启动 `timer` 。

因此 `debounced` 也要修改如下：

```javascript
function debounced(...args) {
  const time = Date.now()
  const isInvoking = shouldInvoke(time)

  lastArgs = args
  lastThis = this
  lastCallTime = time

  if (isInvoking) {
    if (timerId === undefined) {
      return leadingEdge(lastCallTime)
    }
    if (maxing) {
      // Handle invocations in a tight loop.
      timerId = startTimer(timerExpired, wait)
      return invokeFunc(lastCallTime)
    }
  }
  if (timerId === undefined) {
    timerId = startTimer(timerExpired, wait)
  }
  return result
}
```

在 `isInvoking` 为 `false` 时，检测 `timerId` 是否为 `undefined` ，如果是，则重新启动 `timer` ，其实就是增加了这段代码：

```javascript
if (timerId === undefined) {
  timerId = startTimer(timerExpired, wait)
}
```

#### pending

`pending` 方法用来检测 `timer` 是否正在运行中。

源码如下：

```javascript
function pending() {
  return timerId !== undefined
}
```

如果 `timerId` 不为 `undefined`， 则表示 `timer` 正在运行中。

## 参考资料

[探究防抖(debounce)和节流(throttle)](https://github.com/Bowen7/Blog/issues/5)

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 