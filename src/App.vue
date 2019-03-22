<template>
  <div id="app">
    <Todo />
  </div>
</template>

<script>
import Todo from './components/Todo.vue'

export default {
  name: 'app',
  components: {
    Todo
  }
}
</script>

<style lang="scss">
@import './assets/styles/reset.scss';
@import './assets/styles/styles.scss';
</style>
