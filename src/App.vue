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
