<template>
  <div id="app">
    <UserComputer />
  </div>
</template>

<script>
import UserComputer from './components/UserComputer'
export default {
  name: 'App',
  components: {
    UserComputer
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
