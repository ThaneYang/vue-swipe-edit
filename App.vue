<template>
  <div id="app">
    <swipe-edit v-for="item in list" :key="item.name" @tap-swipe="handleSwipe">
      <div class="item">{{item.name}}</div>
      <div slot="edit" class="edit">
        删除
      </div>
    </swipe-edit>
  </div>
</template>

<script>
import swipeEdit from './swipe-edit.vue'

export default {
  name: 'app',
  data() {
    return {
      list: [{
        name: '内容1'
      }, {
        name: '内容2'
      }]
    }
  },
  components: {
    swipeEdit
  },
  methods: {
    handleSwipe() {
      console.log('滑动了')
    }
  }
}
</script>

<style>
*, body {
  margin: 0;
  padding: 0;
}
.item {
  background: #f9f9f9;
  height: 50px;
  line-height: 50px;
}
.edit {
  width: 100px;
  height: 50px;
  line-height: 50px;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
