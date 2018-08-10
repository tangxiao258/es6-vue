## Vue
> Vue是一套用于构建用户界面的**渐进式框架**，vue核心库只关心视图层
[Vue官方文档](https://cn.vuejs.org/v2/guide/)

### 应用
```html
// html部分
<div id="app">
    <div class="user-info" v-if="isLogin">
        <input type="text" v-model="user.name"></input>
        <input type="text" v-model="user.age"></input>
    </div>
    <p>{{count}}</p>
    <button v-on:click="addCount">+</button>
</div>
 
// 直接通过script引入外部js文件
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.17/dist/vue.js"></script>
<script>
    // 创建一个Vue实例
    var vm = new Vue({
        el: '#app',  // 挂载element
        data: {  // 数据对象
            count: 0,
            isLogin: true,
            user: {
                name: 'tangxiao',
                age: '12'
            }
        },
        // 在`methods`对象中定义方法
        methods:{
            addCount: function(){  // 可以简写为addCout() {}
                this.count++
            }
        }
    })
</script>
```


### 生命周期
> [Vue生命周期文档](https://cn.vuejs.org/v2/api/#activated)

![Vue生命周期示意图](https://cn.vuejs.org/images/lifecycle.png)

- beforeCreate  Vue实例初始化之后
- **created** 实例创建完成之后立即被调用。这个阶段，实例已完成以下配置：数据观测（data）,属性和方法的运算,watch/event事件回调，然而挂载阶段暂未开始，即无法访问`$el`（一般我们在这里开始发起后端请求，如果涉及到操作DOM，需移到mounted中）
- beforeMounte  在挂载开始前被调用
- **mounted** `el`挂载成功后调用，但是不保证子组件也一起被挂载
- beforeUpdate  数据更新时调用
- updated  数据更新导致DOM重新渲染后
- activated  keep-alive组件激活时使用
- deactivated keep-alive组件停用时调用
- beforeDestroy 实例销毁之前调用
- **destroyed** Vue实例销毁之后调用
- errorCaptured 捕获子孙组件错误时调用

### 组件
> 组件是Vue最强大的功能之一，组件可以拓展HTML元素，封装可重用的代码，最核心的思想就是重用。

#### 父组件向子组件通信prop
```html
// 父组件  局部注册
new Vue({
    components: {
        'child-component': ChildComponent
    }
})

<child-component v-bind:params="params" v-on:addCount="addCount"></child-component>

// 子组件
new Vue({
    props: ['params'],
    methods:{
        forFather(){
            this.$emit('addCount')
        }
    }
})
```
> 单向数据流，Vue中父子组件之间通过prop形成一个单向的数据流，在实际编程中，应该避免子组件改变prop，从而导致子组件意外改变父组件的状态。

- `prop`传对象特别注意
    - 对象和数组是通过引用传入的，所以在子组件中不能直接通过`=`运算符或ES6的`{...}`扩展运算符对props中的对象进行浅拷贝
    - 使用深拷贝的方式`JSON.parse(JSON.stringify())`

#### 子组件向父组件通信
- `this.$emit`
    - 父组件向子组件传递事件方法，子组件通过`$emit`触发事件，回调给父组件。
    - 
        ```html
        // 父组件
        <template>
            <child @msgFunc="func"></child>
        </template>
        
        // methods
        func(__data){
            console.log(__data)
        }
        
        // 子组件
        <template>
            <button @click="handleClick">点我</button>
        </template>
        
        // methods
        handleClick(){
            this.$emit('msgFunc', {count: 1})
        }

        ```

- `this.$refs`
    - 父组件直接通过`this.$refs`找到对应子组件并获取子组件信息
    - 
        ```js
        <child-component ref="child"></child-component>
        
        // methods:
        this.$refs.child  // 获取到对应子组件，访问其属性即方法
        ```

#### 非父子组件通信
- 推荐使用vuex进行状态管理

### 与其他框架对比
> [官方文档-对比其他框架](https://cn.vuejs.org/v2/guide/comparison.html)

#### React
##### 相同点

- 提供响应式（Reactive）和组件化（Composable）的视图组件
- 将注意力放在核心库（就是想要构建大型应用，你得套个全家桶）

##### 不同点

- React的思想是`HTML in JS`和`CSS in JS`，在React中，一切都是JavaScript
- Vue还是将HTML、CSS、JS分开处理，比较符合原本就是前端人员的编程习惯

#### Angular
##### 相同点

- 在某些语法上比较相似，如`v-if`和`ng-if`，因为vue的作者尤小右之前是从google出来的

##### 不同点

- 相对于Angular，Vue体积更小更灵活
- vue学习曲线更平缓，要用Angular，你必须学会使用TypeScript
- Angular的设计目的就只是针对大型的复杂应用


---