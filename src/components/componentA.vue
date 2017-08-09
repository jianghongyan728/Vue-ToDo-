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


