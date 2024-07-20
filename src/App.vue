<template>
  <h1 align="center" class="mt-4 title">Your to Dolist</h1>
  <Todolist class="tasks container"/>
</template>

<script>
import Todolist from './components/todolist.vue'

export default {
  name: 'App',
  components: {
    Todolist
  }
}
</script>

<style>
#app {

}

.tasks{
    margin-top: 1%;
}
.title{
  color: white;
  font-weight: 800;
  font-family: Impact, Haettenschweiler, 'Arial Narrow Bold', sans-serif;
}
</style>
