## 箭头函数
### 基本用法

```js
sum = (a, b) => {
    return a + b
}

list.map((item, index) => {
    console.log(item)
})
```

#### 箭头函数中的this
在箭头函数出现之前，每个新定义的函数都有自己的this值

```js
function Person(){
    this.age = 0
    
    setInterval(function(){
        // 此处`this`与上一个`this`并不相同
        this.age++
    } ,1000)
}
// 因此我们常看到下面的代码
function Person(){
    var __this = this
    __this.age = 0
    
    setInterval(function() {
        // 此处`this`与上一个`this`并不相同
        __this.age++
    } ,1000)
}
```

箭头函数不会创建自己的`this`，它只会从自己的作用域链的上一层继承this

```js
// 上面的代码可以修改为
function Person(){
    this.age = 0
    
    setInterval(() => {
        // `this`正确的指向person对象
        this.age++
    } ,1000)
}
```

### 使用注意点
- 函数题内的`this`对象，就是定义时所在的对象，而不是使用时所在的对象
- 不可以当作构造函数，也就是说，不可以使用`new`命令，否则会抛出一个错误
- 不可以使用`arguments`对象，该对象在函数体内不存在。如果要用，可以用`reset`参数代替

