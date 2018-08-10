## 参数处理
### 默认参数值
> 函数默认参数允许在没有值或`undefined`被传入时使用默认参数。

- ES5添加默认参数

```js
function Person(name, age){
    name = name || '张三'
    age = age || 18
    // 上面的代码并不严谨，只是我们经常这样写
    // 也可以这样写
    name = (typeof name !== 'undefined') ? name : '张三'
    age = (typeof age !== 'undefined') ? age : 18
}

Person('tangxiao')  // name='tangxiao',age=18
```

- ES6写法

```js
function Person(name = "张三", age = 14){
    
}
```

### 剩余参数
> **剩余参数**语法允许我们将一个不定数量的参数表示为一个数组。

```js
function sum(...theArgs){
    return theArgs.reduce((pre, cur) => {
        return pre + cur
    })
}

console.log(sum(1, 2, 3))  // 6
```

### 展开运算符

```js
function sum(x, y, z){
    return x + y + z
}

const numbers = [1, 2, 3]

console.log(sum(...numbers))  // 6
```

- 函数调用

```js
myFunction(...iterableObj)
```

- 字面量数组构造或字符串

```js
let numbers = [1, 2, 3]

let newArr = [...numbers, 4, 5]
// [1, 2, 3, 4, 5]
```

- 构造字面量对象，进行克隆或属性拷贝

```js
let defaultObj = {
    name: 'tangxiao',
    age: 16
}

let newObj = {name: 'new'}

let objClone = {...defaultObj, ...newObj}
// {name: 'new', age: 16}
// 此处为浅拷贝
```