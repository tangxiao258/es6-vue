## 对象属性加强
### 属性定义

```js
var a = 'foo'
var b = 42
var c = {}

// ES5
var o = {
    a: a,
    b: b,
    c: c
}

// ES6
var o = {a, b, c}
```

### 方法定义

```js
// ES5
var o = {
    property: function(params){
        ...
    }
}

// ES6 简写，省去`function`关键字
var o = {
    property(params){
        ...
    }
}
```

### 计算属性名

```js
var param = 'size'
var config = {
    [param]: 12,
    ['mobile' + param.charAt(0).toUpperCase() + param.slice(1)]: 4
}

console.log(config)  // {size: 12, mobileSize: 4}
```