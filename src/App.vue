<template>
  <div>
    <TodoNav/>
    <TodoList/>
    <TodoFooter/>
  </div>
</template>

<script>
import TodoNav from './components/TodoNav'
import TodoList from './components/TodoList'
import TodoFooter from './components/TodoFooter'

export default {
  name: 'app',
  components: {
    TodoNav,
    TodoList,
    TodoFooter
  }
}
</script>

<style>

</style>
