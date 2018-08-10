## 作用域
### 块级作用域
- **ES5** 只有全局作用域和函数作用域
- **ES6** 有块级作用域，使用`{}`表示

在ES5中，我们经常看到这样的代码
```js
// 使用立即执行函数，因为我们不想让var声明的变量变为全局变量
!function(){
    // 使用function，里面声明的变量只在函数作用域内有效
    var name = "tangxiao"
    
}.call()

// 或者这种
(function(){

})()
```
ES6写法
```js
// ES6有块级作用域，使用`{}`表示
{
    var name = "tangxiao"
}
```

### let
- **ES5** 使用 ==`var`== 声明变量，造成变量提升，尤其在`for`循环当中
- **ES6** 新增 ==`let`== 声明变量，无变量提升，作用域在代码块内\


**使用`var`**
```js
var a = [];
for (var i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 10

// var的变量提升导致全局只有一个i变量
// 每次循环，全局变量i都会改变
// 也就是每次的console.log(i)都是指向全局变量i
// 最后打印出就是10

// 以上代码相当于
var a;
var i;

a = [];

for (i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 10
```

**使用`let`**
```js
var a = [];
for (let i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 6

// 变量i是let声明的，当前的i只在本轮循环中有效
// 所以每次的i都是一个新变量
```

### const
- `const`用于常量的声明
- 使用`const`声明常量，一旦赋值，不可修改
- 声明之后必须要赋值

```js
const EVENTS = 'playMusic'
```