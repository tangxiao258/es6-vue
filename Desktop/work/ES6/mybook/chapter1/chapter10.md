## 原有内置对象API增强

### Object.assign()
> Object.assign()方法用于将所有可枚举属性的值从一个或多个源对象复制到目的对象。它将返回目标对象 

```js
let defaultObj = {
    name: 'tangxiao',
    age: 18
}

let newObj = Object.assign(defaultObj, {sex: 'female'})
console.log(newObj)  //{name: 'tangxiao', age: 18, sex: 'female'}
```

### Array.from()
> Array.from()用于将两类对象转化为真正的数组（类数组/可遍历的对象）

```js
let arrayLike = {
    '0': 'a',
    '1': 'b',
    '2': 'c',
    length: 3
};

// ES5的写法
var arr1 = [].slice.call(arrayLike); // ['a', 'b', 'c']

// ES6的写法
let arr2 = Array.from(arrayLike); // ['a', 'b', 'c']
```

常见的伪数组
- 通过`document.querySelectorAll()`获取到的dom对象
- `arguments`对象

### Array.of()
> Array.of()方法创建一个具有可变数量参数的新数组实例。

```js
Array.of(7);       // [7] 
Array.of(1, 2, 3); // [1, 2, 3]

Array(7);          // [ , , , , , , ]
Array(1, 2, 3);    // [1, 2, 3]
```


