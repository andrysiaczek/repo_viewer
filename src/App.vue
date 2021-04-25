<template>
  <RepoViewer class='app'/>
</template>

<script>
import RepoViewer from './components/RepoViewerApp.vue'

export default {
  components: {
    RepoViewer
  }
}
</script>

<style>
  .app {
    padding: 0;
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    height: 100%;
  }
</style>