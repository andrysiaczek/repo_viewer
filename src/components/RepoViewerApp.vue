<template>
  <div class='container'>
    <div class='header'>
      <h1>RepoViewer</h1>
      <!-- <img src='src/assets/git-repository-logo.png' alt='git logo' /> -->
    </div>
    <form class='form'>
      <label for='username'>Username: </label>
      <input id='username' v-model='username' type='text' name='username'>
    </form>
    <div class='content'>
      <Repositories v-if='!loading && !notFound' :repos='repos'/>
      <span v-if='!loading && notFound'>
        <p>This user doesn't exist on GitHub</p>
      </span>
    </div>
  </div>
</template>

<script>
import Repositories from './RepoList.vue';
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
        loading: true,
        error: false,
        notFound: false,
      }
    },
    watch: {
      username() {
        this.loading = true;
        if (this.username.length === 0){
          this.notFound = false;
          this.error = false;
        }
        else {
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
          }
      },
    },
}
</script>

<style>
  .container {
    display: flex;
    flex-direction: column;
    width: 100%;
    background-color: whitesmoke;
  }

  .header {
    padding: 60px;
    background-color: rgb(24, 25, 33);
    color: thistle;
    width: 100%;
    align-self: flex-start;
  }

  .form {
    background-color: rgba(120, 114, 196, 0.453);
    width: 100%;
    padding: 20px;
    display: grid;
  }

  .content {
    background-color: rgb(255, 255, 255);
    width: 100%;
    padding: 20px;
  }
</style>