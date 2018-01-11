<template>
  <div id="app">
    <ScheduleList></ScheduleList>
  </div>
</template>

<script>
import ScheduleList from './components/ScheduleList'

export default {
  name: 'app',
  components: {
    ScheduleList
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
