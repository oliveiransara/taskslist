<template>
  <div class="app">
    <header>Minhas tarefas</header>
    <TaskView />
  </div>
</template>

<script>
import TaskView from './components/Task';

export default {
  name: 'App',
  components:{
    TaskView,
  },
}
</script>

<style>
header{
  display: flex;
  justify-content: center;
  align-items: center;
  height: 80px;
  font-size: 40px;
  color: #000;
}
</style>
