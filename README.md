## Vue做的ToDo小例子
##### 今天用vue做有个小例子
##### 首先要去vue的官网去看看，https://cn.vuejs.org/   官网唯一的好处就是中文版的
##### 我们先简单的认识一下vue再来进行安装
![看这里](https://github.com/jianghongyan728/Vue-ToDo-/blob/master/vue.png);
#### 命令行安装
```
# 全局安装 vue-cli
$ npm install --global vue-cli
# 创建一个基于 webpack 模板的新项目
$ vue init webpack my-project
# 安装依赖，走你
$ cd my-project
$ npm install
$ npm run dev
```
> 安装vue init webpack my-project的时候会出现几条命令，都选择no就可以了
![看这里](https://github.com/jianghongyan728/Vue-ToDo-/blob/master/ming.png);

> 译者注：对于大陆用户，建议将 npm 的注册表源设置为国内的镜像，可以大幅提升安装速度。可以用中国淘宝的镜像cnpm
##### 首先我们看一下一个小例子，然后在做Todo的小例子
![看这里](https://github.com/jianghongyan728/Vue-ToDo-/blob/master/li.png);
##### 接下来看看我们的代码吧
###### App.vue

```
<template>
  <div id="app">
    <h1 v-text="title"></h1>
    <input v-model="newItem" v-on:keyup.enter="addNew" />
    <ul>
      <li v-for="item in items" v-bind:class="{finished: item.isFinished}"v-on:click="toggleFinish(item)">
        {{item.label}}
      </li>
    </ul>
    <p>child tells me: {{ childWorlds }}</p>
    <component-a msgfromfather = "you die!!" 
    v-on:child-tell-me-something="listenToMyBoy"></component-a>
  </div>
</template>

<script>
import Store from './store'
import ComponentA from './components/componentA'

export default {
  data: function(){
    return {
      title: 'this is a todo list',
      items: Store.fetch(),
      newItem:'',
      childWorlds: ''
    }
  },
  components: { ComponentA },
  watch:{
    items: {
      handler: function(items){
        Store.save(items)
      }, 
      deep: true
    }
  },
  events: {
    'child-tell-me-something': function (msg) {
      this.childWorlds = msg;
    }
  },
  methods:{
    toggleFinish:function(item){
      item.isFinished =! item.isFinished
    },
    addNew: function(){
      this.items.push({
        label: this.newItem,
        isFinished:false
      })
      this.newItem=''
      this.$broadcast('onAddnew',this.items)
    },
    listenToMyBoy: function (msg) {
      this.childWorlds = msg;
    }
  }
}
</script>

<style>
.finished{
  text-decoration:underline;
}
p{
  color:blue;
}
h1{
  color:green;
}
</style>

```

###### components下的componentA.js

```
<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <h1>{{ msgfromfather }}</h1>
    <button v-on:click="onClickMe">open mouse!</button>
  </div>
</template>

<script>
export default {
  name: 'hello',
  data: function () {
    return {
      msg: 'hello from component a!'
    }
  },
  props:['msgfromfather'],
  events: {
    'onAddnew':function (items){

    }
  },
  methods: {
    onClickMe: function () {
      this.$emit('child-tell-me-something', this.msg);
    }
  }
}
</script>

```
###### store.js

```
const STORAGE_KEY = 'todos-vuejs'
export default{
    fetch: function () {
        return JSON.parse(window.localStorage.getItem(STORAGE_KEY) || '[]')
    },
    save: function (items) {
        window.localStorage.setItem(STORAGE_KEY,JSON.stringify(items))
    }
}
```
###### 接下来看看效果吧！！！！
![看这里](https://github.com/jianghongyan728/Vue-ToDo-/blob/master/xiao.gif);

#### 接下来我们就做一个小总结

- new一个vue对象的时候NIIT可以设置它的属性，其中最重要的三个，分别是data，methods，watch

- 其中data代表vue对象的数据，methods代表vue对象的方法，watch设置了对象监听的方法。

- Vue对象里的设置通过html指令进行关联

- 重要的指令包括
1. v-text渲染数据
2. v-if控制显示
3. v-on绑定事件
4. v-for循环渲染
