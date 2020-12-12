<template>
  <div id="app">
    <plan />
  </div>
</template>

<script>
import plan from './components/plan.vue'

export default {
  name: 'App',
  components: {
    plan
  }
}
</script>

<style lang="less">
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: Impact, Haettenschweiler, 'Arial Narrow Bold', sans-serif, '微软雅黑';
}
</style>
