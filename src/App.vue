<template>
  <div class='app'>
    <h1>Git Repos App</h1>
    <GitReposApp />
  </div>
</template>

<script>
import GitReposApp from './components/UserReposApp.vue'

export default {
  components: {
    GitReposApp
  }
}
</script>

<style>
.app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>