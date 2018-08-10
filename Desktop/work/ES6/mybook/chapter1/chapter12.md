## 类

```js
// JavaScript 语言中，生成实例对象的传统方法是通过构造函数
function Point(x, y) {
  this.x = x;
  this.y = y;
}

Point.prototype.toString = function () {
  return '(' + this.x + ', ' + this.y + ')';
};

var p = new Point(1, 2);

// ES6引入Class(类)这个概念
// 使用class语法糖，让对象圆形的写法更加清晰
// 上面代码改写
//定义类
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }
}
```