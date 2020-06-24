<template>
  <div id="app">
    <ApolloBasic msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import ApolloBasic from './components/ApolloBasic.vue'

export default {
  name: 'App',
  components: {
    ApolloBasic
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
