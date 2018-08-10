## 解构赋值
### 数组解构

- 基本用法

```js
var foo = ['one', 'two', 'three']

var [a, b, c] = foo

console.log(a)  // 'one'
console.log(b)  // 'two'
console.log(c)  // 'three'
```

- 交换变量

```js
var a = 1
var b = 3

[a, b] = [b, a]
a // 3
b // 1
```

- 解析一个从函数返回的数组

```js
function f(){
    return [1, 2]
}
var [a, b] = f()
```

- 忽略某些返回值

```js
function f(){
    return [1, 2, 3]
}
var [a, , b] = f()
a  // 1
b  // 3
```


### 对象解构
- 基本用法

```js
var o = {p: 42, q: true}
var {p, q} = o

console.log(p)  // 42
console.log(q)  // true
```

- 给新的变量名赋值

```js
var o = {p: 42, q: true};
var {p: foo, q: bar} = o;
 
console.log(foo); // 42 
console.log(bar); // true

```

### 参数匹配
```js
// ES5我们经常会遇到这样的代码
function fn(__params){
    var user = __params.user
    var age = __params.age
}

// ES6
function fn({user, age}){
    ...
}
```