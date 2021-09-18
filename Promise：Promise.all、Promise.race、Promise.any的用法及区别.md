# Promise：Promise.all、Promise.race、Promise.any的用法及区别

在项目开发过程中经常需要通过异步编程来实现功能，此时就需要我们了解`Promise`.

## `Promise`

`Promise` 是异步编程的一种解决方案，比传统的解决方案**回调函数和事件**更合理和更强大。

有了`Promise`对象，就可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数。

一个`Promise`的当前状态必须为以下三种状态中的一种：等待态（`Pending`）、执行态（`Fulfilled`）和拒绝态（`Rejected`），状态的改变只能是单向的，且变化后不可在改变。

一个`Promise`必须提供一个 `then` 方法以访问其当前值、终值和据因。

`promise.then(onFulfilled, onRejected)`回调函数只能执行一次，且返回 `promise` 对象

`Promise`的每个操作返回的都是`Promise`对象，可支持链式调用。

通过 `then` 方法执行回调函数，`Promise`的回调函数是放在事件循环中的微队列。

`Promise`的具体用法如下：

```js
function fn(){
     return new Promise((resolve, reject)=>{
         成功时调用 resolve(数据)
         失败时调用 reject(错误)
     })
 }
 fn().then(success1, fail1).then(success2, fail2)
```

## `Promise.all`

`Promise.all()`方法用于将多个 `Promise` 实例，包装成一个新的 `Promise` 实例。

`Promise.all()`全部子实例都成功才算成功，有一个子实例失败就算失败。

```js
Promise.all([promise1, promise2]).then(success1, fail1)
promise1`和`promise2`都成功才会调用`success1
```

## `Promise.race`

`Promise.race()`方法也是将多个 `Promise` 实例，包装成一个新的 `Promise` 实例。

`Promise.race()`rece是赛跑机制，要看最先的promise子实例是成功还是失败。

```js
Promise.race([promise1, promise2]).then(success1, fail1)
promise1`和`promise2`只要第一个成功就会调用`success1
```

## `Promise.any`

`Promise.any()`方法同样是将多个 `Promise` 实例，包装成一个新的 `Promise` 实例。

`Promise.any()`有一个子实例成功就算成功，全部子实例失败才算失败。

```js
Promise.race([promise1, promise2]).then(success1, fail1)
promise1`和`promise2`只要有一个成功就会调用`success1
```



> 总结：`Promise.all()` 方法是 `&&` 的关系；`Promise.any()` 方法是 `||` 的关系； `Promise.race()`方法是 `赛跑机制` ；
