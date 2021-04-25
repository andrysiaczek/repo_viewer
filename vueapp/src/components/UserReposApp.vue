<template>
  <div class="container">
    <form class="app">
      <p>
        <label for="username">Username</label>
        <input id="username" v-model="username" type="text" name="username">
      </p>
    </form>
    <span v-if="!loading && !notFound">
      <Repositories :repos='repos'/>
    </span>
    <span v-if="!loading && notFound">
      <p>This user doesn't exist on GitHub</p>
    </span>
  </div>
</template>

<script>
import Repositories from './ReposList.vue';
import axios from 'axios';

export default {
    components: {
        Repositories
    },
    data() {
      return {
        username: null,
        repos: null,
        sortedRepos: null,
        get: true,
        loading: true,
        error: false,
        notFound: false,
      }
    },
    watch: {
      username() {
        axios
        .get('https://api.github.com/users/' + this.username + '/repos')
        .then(res => {
          this.repos = res.data;
          this.notFound = false;
          this.error = false;
        })
        .catch( error =>  {
          if (error.response.status === 404) {
            this.notFound = true;
          }
          this.error = true;
        })
        .finally( () => this.loading = false )
      },
    },
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
