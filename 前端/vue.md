# vue-cli

Vue-CLI是Vue.js的脚手架，用来自动生成`vue.js + webpack`的项目模板。



# vue-router

官网： https://router.vuejs.org/zh/

## 使用

1. 在命令行中进入 vue 的项目目录里，运行命令 `npm install vue-router --save` 来进行安装 ；
1. 创建目录`src/router`，创建文件`index.js`，在文件中添加如下代码：
```
import Vue from 'vue';
import VueRouter from 'vue-router';
import iView from 'view-design';

Vue.use(VueRouter);

// 1. 定义 (路由) 组件。
// 可以从其他文件 import 进来
const routes = [
{path:'/',component: () =>import ('@/view/home')},
];

const router = new VueRouter({
routes
})
//跳转前
router.beforeEach((to, from, next) => {
iView.LoadingBar.start();
next() // 跳转
});

//跳转后
router.afterEach(to => {
iView.LoadingBar.finish();
window.scrollTo(0, 0);
});

export default router;
```
1. 在main.js中引入：
```
import Vue from 'vue';
import App from '@/App';
import router from '@/router';

import iView from 'view-design';
import 'view-design/dist/styles/iview.css'; // 使用 CSS
Vue.use(iView)

new Vue({
el: '#app',
router:router,
render: h => h(App)
});
```
1. 在 App.vue 中添加`<router-view></router-view>`，代码如下：
```
<template>
<div id="app">
<router-view/>
</div>
</template>

<script>
export default {
name: 'App',
created() {
try {
document.body.removeChild(document.getElementById('appLoading'))
setTimeout(function() {
document.getElementById('app').style.display = 'block';
}, 500)
} catch (e) {

}
}
}
</script>

<style lang="less">
.size{
width: 100%;
height: 100%;
}
html,body{
.size;
overflow: hidden;
margin: 0;
padding: 0;
}
#app {
.size;
}
</style>
```
5.

# axios

```
npm install axios
```
# qs

```
npm i qs
```
# less

```
npm i less less-loader -S
```
更改配置文件`build/webpack.base.conf.js`

在`module.export`暴露的对象中，为`module`的`rules`添加如下配置：

```
{
test: /\.less$/,
loader: "style-loader!css-loader!less-loader",
}
```
在`style`标签上添加`lang`属性，指定使用的样式语法；

```
<style scoped lang="less">
</style>
```
注释: `scoped`为只在当前组件中有效

# iview

```
npm install view-design --save
```
main.js中

```
import iView from 'view-design';
import 'view-design/dist/styles/iview.css'; // 使用 CSS
Vue.use(iView)
```
# echarts

```
npm install echarts -S
```
在main.js中

```
// 引入echarts
import echarts from 'echarts'
Vue.prototype.$echarts = echarts
```
使用时，如 在Echarts.vue中

```
<div id="myChart" :style="{width: '300px', height: '300px'}"></div>
```
引入echarts

```
//引入echarts
import echarts from 'echarts';
//import '../../node_modules/echarts/map/js/world.js' //引入世界地图
import '../../../node_modules/echarts/map/js/china.js' // 引入中国地图数据
```
创建map-option.js

```
export default { // 进行相关配置
backgroundColor: "#02AFDB",
tooltip: {}, // 鼠标移到图里面的浮动提示框
dataRange: {
show: false,
min: 0,
max: 1000,
text: ['High', 'Low'],
realtime: true,
calculable: true,
color: ['orangered', 'yellow', 'lightskyblue']
},
geo: { // 这个是重点配置区
map: 'china', // 表示中国地图
roam: true,
label: {
normal: {
show: true, // 是否显示对应地名
textStyle: {
color: 'rgba(0,0,0,0.4)'
}
}
},
itemStyle: {
normal: {
borderColor: 'rgba(0, 0, 0, 0.2)'
},
emphasis: {
areaColor: null,
shadowOffsetX: 0,
shadowOffsetY: 0,
shadowBlur: 20,
borderWidth: 0,
shadowColor: 'rgba(0, 0, 0, 0.5)'
}
}
},
series: [{
type: 'scatter',
coordinateSystem: 'geo' // 对应上方配置
},
{
name: '启动次数', // 浮动框的标题
type: 'map',
geoIndex: 0,
data: [{
"name": "北京",
"value": 599
}, {
"name": "上海",
"value": 142
}, {
"name": "黑龙江",
"value": 44
}, {
"name": "深圳",
"value": 92
}, {
"name": "湖北",
"value": 810
}, {
"name": "四川",
"value": 453
}]
}
]
}
```
方法中使用：

```
let myChart =echarts.init(this.$refs.myEchart); //这里是为了获得容器所在位置
window.onresize = myChart.resize;
myChart.setOption(option);
```
完整vue：

```
<template>

<div class="layout">
<Layout>
<Header class="layout-header">
<h1>图书馆服务地图</h1>
</Header>
<Content class="layout-content">
<div :style="{height:'100%',width:'100%'}" ref="myEchart"></div>
</Content>
<Footer class="layout-footer">版权所有：北京南天软件有限公司</Footer>
</Layout>
</div>
</template>

<script>
import echarts from 'echarts';
import '../../../node_modules/echarts/map/js/china.js' // 引入中国地图数据
import option from "./map-option.js";

export default {
name: 'Home',
data() {
return {
chart: null
};
},
mounted() {
this.mapEchartsInit();
},
methods: {
mapEchartsInit() {
let myChart =echarts.init(this.$refs.myEchart); //这里是为了获得容器所在位置
window.onresize = myChart.resize;
myChart.setOption(option);
}
},
beforeDestroy() {
if (!this.chart) {
return;
}
this.chart.dispose();
this.chart = null;
},
}
</script>
<style scoped  lang="less">
.layout{
border: 1px solid #d7dde4;
background: #f5f7f9;
position: relative;
border-radius: 4px;
overflow: hidden;
height: 100%;
width:100%;

.ivu-layout{
height: 100%;
width:100%;
}
.layout-header{
background:#d7dde4;
text-align: center;
position: fixed;
width: 100%;
}
.layout-content{
margin:0;
margin-top: 64px;
background:#fff;
}
.layout-footer{
text-align: center;
}
}
</style>
```
