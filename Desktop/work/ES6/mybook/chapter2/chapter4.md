## vue-router
> Vue-router是Vue.js的官方路由管理器 [官方文档](https://router.vuejs.org/zh/)

### 单页面应用（SinglePage Web Application, SPA）
只有一张Web页面的应用，是一种从Web服务器加载的富客户端，单页面跳转仅刷新局部资源，公共资源（js、css）仅需加载一次，常用于后台管理系统，购物网站等。

![单页面应用示例图](https://user-gold-cdn.xitu.io/2017/11/17/15fc93562b418a6e?imageslim)

### 传统的多页面应用
多页面跳转需刷新所有的资源，每个公共资源（js、css）需选择性重新加载，常用于app或客户端等。

![image](https://user-gold-cdn.xitu.io/2017/11/17/15fc93684b5f10e1?imageslim)

### 具体对比分析
 -- | 单页面应用（SinglePage Web Application, SPA） | 多页面应用（MultiPage Application, MPA）
---|---|---
组成 | 一个外壳页面和多个页面片段组成 | 多页完整页面组成
资源公用（js、css） | 公用，只在外壳部分加载 | 不通用，每个页面都需要加载
刷新方式 | 页面局部刷新或更改 | 整页刷新
url模式 | a.com/#/pageone | a.com/pageone.html
用户体验 | 页面片段间的切换快，用户体验良好 | 页面切换加载缓慢，流畅度不够，用户体验比较差
转场动画 | 容易实现 | 无法实现
数据传递 | 容易 | 依赖url传参、或者cookie、localStorage等
搜索引擎优化（SEO） | 需要单独方案、实现较为困难，不利于SEO检索，可利用服务器端渲染（SSR）优化 | 实现方法简易
试用范围 | 高要求体验度、追求界面流畅的应用 | 适用于追求高度支持搜索引擎的应用
开发成本 | 较高，常需借助专业的框架 | 较低，但页面重复代码多
维护成本 | 相对容易 | 相对复杂

#### 单页面路由实现基本原理(Router)
**通过url上的锚点**

url上的hash本意是用来做锚点的，方便用户在一个很长的文档里面进行上下导航，用来做SPA的路由控制器并不是它的本意。然而hash满足这么一种特性：改变url的同时，不刷新页面，再加上浏览器也提供onhashchange这样的事件监听，因此，hash能用来做路由控制器。

### 安装
`npm install vue-router`

### HTML
```js
<!-- 使用router-link组件来导航 -->
<!-- 通过`to`属性指定链接 -->
<!-- <router-link>默认会被渲染成一个a标签 -->
<router-link to="/page1">页面一</router-link>
<router-link to="/page2">页面二</router-link>

<!-- 路由出口 -->
<!-- 路由匹配到的组件将渲染在这里 -->
<router-view></router-view>
```
### JavaScript
```js
// 使用import导入Vue和VueRouter
import Vue from 'vue';
import VueRouter from 'vue-router';

// 从其他文件导入（路由）组件
import page1 from '../components/page1';
import page2 from '../components/page2';

// 调用Vue.use方法
Vue.use(VueRouter);

// 定义路由
const routes = [
    {path: '/', component: page1},
    {path: '/page1', component: page1},
    {path: '/page2', component: page2}
]

// 创建router实例，然后传`routes`配置
const router = new VueRouter({
    routes  // （缩写）相当于routes: routes
})

export default router;

// main.js
new Vue({
    router,  // (缩写)相当于router: router
})
```

### 使用
#### 通过`router`或`this.$router`访问路由器
> 一般我们都是通过全局注册的路由，即`Vue.use(VueRouter)`，所以可以通过`this.$router`访问路由器

- router.push()
    - **字符串** `router.push('page1')`
    - **对象** `router.push({path: '/page1'})`
    - **命名的路由**`router.push({name: 'page1', params: {userId: 123}})`
    - **带查询参数**`router.push({path: '/page1', query: { plan: 'private' }})`
- router.go()
    - `router.go(-1)`后退一步
    - `router.go(1)`前进一步
    
    
#### 通过`route`或`this.$route`访问当前路由
> 通过全局注册的路由，即`Vue.use(VueRouter)`，可通过`this.$route`访问当前路由

- `route.params`获取当前组件的参数
- `route.path` 当前路由路径
- `rour.name` 当前路由name


#### 通过组件props组件之间传值解耦
> 由于使用`route.params`获取参数需高度依赖于路由，因此所以可以通过组件之间传值`props`解耦

```js
<my-component :params="{userId: 'admin'}"></my-component>
```
