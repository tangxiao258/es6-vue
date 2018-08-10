## Promise
### 回调地狱

```js
function fn1() {
  setTimeout(()=>{
    console.log('fn1')
  }, 1000)
}

function fn2() {
  setTimeout(()=>{
    console.log('fn2')
  }, 1000)
}

function fn3() {
  setTimeout(()=>{
    console.log('fn3')
  }, 1000)
}

```
以上代码如何实现：1秒钟后执行fn1，再过1s之后输出fn2，再过1s后输出fn3

改写：
```js
function fn1(callback) {
  setTimeout(()=>{
    console.log('fn1')
    callback()
    
    // 如果需要判断是否有回调函数需要执行，可以这样写
    // if(typeof callback === 'function') callback()
  }, 1000)
}

function fn2(callback) {
  setTimeout(()=>{
    console.log('fn2')
    callback()
  }, 1000)
}

function fn3() {
  setTimeout(()=>{
    console.log('fn3')
  }, 1000)
}


fn1(() => {
    fn2(() => {
        fn3()
    })
})
```
这就是 **回调地狱**

### 什么是Promise
Promise是一个对象，该对象存储一个状态，这个状态是可以随着内部的执行转化的。为以下三种状态之一：
- 等待态（Pending）
- 完成态（Fulfilled）
- 拒绝态（Rejected）

在使用时，我们先设置好等待状态从pending变成fulfilled和rejected的预案
- 当成功后我们做什么
- 当失败时我们做什么

Promise启动之后
- 当满足成功的条件时
    - 状态从`pending`变为`fulfilled`（==执行resolve==）
- 当满足失败条件时
    - 状态从`pending`变成`rejected`（==执行reject==）

### 基本用法

```js
// 定义一个Promise实例
const promise = new Promise((resolve, reject) => {
    // ... some code
    
    if(/*异步操作成功*/){
        resolve(value)
    }else{
        reject(value)
    }
})

// 使用
promise.then((value) =>{
    // success
    return '1'
}, (error) => {
    // failure
}).then((value2) => {
    console.log(value2)  // '1'
    // 链式调用，此处的value为上一个then的success状态后的成功的返回值
})
```

### Promise.prototype.then/Promise.prototype.catch
```js
function getIp() {
  var promise = new Promise((resolve, reject) => {
    var xhr = new XMLHttpRequest()
    xhr.open('GET', 'https://easy-mock.com/mock/5ac2f80c3d211137b3f2843a/promise/getIp', true)
    xhr.onload = () => {
      var retJson = JSON.parse(xhr.responseText)  // {"ip":"58.100.211.137"}
      resolve(retJson.ip)
    }
    xhr.onerror = () => {
      reject('获取IP失败')
    }
    xhr.send()
  })
  return promise
}

function getCityFromIp(ip) {
  var promise = new Promise((resolve, reject) => {
    var xhr = new XMLHttpRequest()
    xhr.open('GET', 'https://easy-mock.com/mock/5ac2f80c3d211137b3f2843a/promise/getCityFromIp?ip='+ip, true)
    xhr.onload = () => {
        var retJson = JSON.parse(xhr.responseText)  // {"city": "hangzhou","ip": "23.45.12.34"}
      resolve(retJson.city)
    }
    xhr.onerror = () => {
      reject('获取city失败')
    }
    xhr.send()
  })
  return promise
}

function getWeatherFromCity(city) {
  var promise = new Promise((resolve, reject) => {
    var xhr = new XMLHttpRequest()
    xhr.open('GET', 'https://easy-mock.com/mock/5ac2f80c3d211137b3f2843a/promise/getWeatherFromCity?city='+city, true)
    xhr.onload = () => {
      var retJson = JSON.parse(xhr.responseText)   //{"weather": "晴天","city": "beijing"}
      resolve(retJson)
    }
    xhr.onerror = () => {
      reject('获取天气失败')
    }
    xhr.send()
  })
  return promise
}

getIp().then((ip) => {
  return getCityFromIp(ip)
}).then((city) => {
  return getWeatherFromCity(city)
}).then((data) => {
  console.log(data)
}).catch((e) => {
  console.log('出现了错误', e)
})
```

### Promise.all
```js
function getCityFromIp(ip) {
  var promise = new Promise((resolve, reject) => {
    var xhr = new XMLHttpRequest()
    xhr.open('GET', 'https://easy-mock.com/mock/5ac2f80c3d211137b3f2843a/promise/getCityFromIp?ip='+ip, true)
    xhr.onload = () => {
      var retJson = JSON.parse(xhr.responseText)  // {"city": "hangzhou","ip": "23.45.12.34"}
      resolve(retJson)
    }
    xhr.onerror = () => {
      reject('获取city失败')
    }
    xhr.send()
  })
  return promise
}

var p1 = getCityFromIp('10.10.10.1')
var p2 = getCityFromIp('10.10.10.2')
var p3 = getCityFromIp('10.10.10.3')


//Promise.all, 当所有的 Promise 对象都完成后再执行
Promise.all([p1, p2, p3]).then(data=>{
  console.log(data) // 返回一个数组
})
```

### 对于开始回调地狱的解决
```js
function fn1() {
  return new Promise((resolve, reject)=>{
    setTimeout(()=>{
      console.log('fn1...')
      resolve()
    }, 1000)    
  })
}

function fn2() {
  return new Promise((resolve, reject)=>{
    setTimeout(()=>{
      console.log('fn2...')
      resolve()
    }, 1000)    
  })
}

function fn3() {
  return new Promise((resolve, reject)=>{
    setTimeout(()=>{
      console.log('fn3...')
      resolve()
    }, 1000)    
  })
}

function onerror() {
  console.log('error')
}

fn1().then(fn2).then(fn3).catch(onerror)
```

### async解决异步问题
> ES2017 标准引入了 async 函数，用于更方便的处理异步。 

```js
function delay(time) {
  return new Promise((resolve,reject) => {
    setTimeout(()=>{
      if(Math.random()> 0.1){
        resolve(time)
      }else{
        reject('error..')
      }
    }, time)
  })
}

async function fn() {
  console.log('start')
  let time = await delay(1000)
  console.log(`${time}ms passed`)
  let time2 = await delay(3000)
  console.log(`${time2}ms passed`)
}
fn().catch(err=>console.log(err))
```