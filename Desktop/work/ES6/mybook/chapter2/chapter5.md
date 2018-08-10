## vuex
> Vuex是一个专为Vue.js应用程序开发的状态管理模式。

### 主要解决的问题
- 多个视图依赖同一个状态（比如用户登录状态，国际化中的语言状态）
- 来自不同视图的行为需要变更同一状态

### 适用场景
> 中大型单页应用

### 安装
`npm install vuex`

### 一个简单的Store
```
const store = new Vuex.Store({
    state: {
        count: 0
    },
    mutations: {
        addCount (state) {
            state.count++
        },
        reduceCount (state){
            state.count--
        }
    }
})
```

### 使用方式
- 使用`this.$store.state`获取状态对象
- 使用`this.$store.commit('addCount')`来触发状态变更

### 在Vue组件中获取Vuex状态
> 由于Vuex的状态存储是响应式的，从store实例中获取状态的最简单的方法就是在**计算属性**返回某个状态
```js
export default {
    name: 'page1',
    computed:{
        count(){
            // 每当store.state.count变化时，都会重新求去计算属性，并且触发更新相关的DOM
            return this.$store.state.count
        }
    },
    methods:{
        addCount(){
            this.$store.commit('addCount')
        },
        reduceCount(){
            this.$store.commit('reduceCount')
        }
    }
}
```

#### `mapState`辅助函数
当一个组件需要获取多个状态的时候，使用`mapState`和对象展开运算符(`mapState函数返回的是一个对象`)
```js
computed:{
    localComputed(){/*...*/},
    // 使用对象展开运算符将此对象混入到外部对象中
    ...mapState([
        'count'
    ])
}
```