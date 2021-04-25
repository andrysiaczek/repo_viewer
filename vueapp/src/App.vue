<template>
  <div class="container">
    <form class="app">
      <p>
        <label for="username">Username</label>
        <input id="username" v-model="username" type="text" name="username">
      </p>
    </form>
    <span v-if="!loading && !notFound">
      <h3>Repositories:</h3>
      <table class="table">
          <thead>
          <tr>
              <th scope="col">Name</th>
              <th scope="col">Stars</th>
          </tr>
          </thead>
          <tbody v-if="repos && Object.keys(repos).length != 0">
          <tr v-for="repo in sort(repos)" v-bind:key="repo.id"> 
              <th scope="row">{{repo.name}}</th>
              <td>{{repo.stargazers_count}}</td>
          </tr>
          </tbody>
          <tbody v-else-if="repos && Object.keys(repos).length === 0">
            <p>User doesn't have any repos.</p>
          </tbody>
      </table>
    </span>
    <span v-if="!loading && notFound">
      <p>This user doesn't exist on GitHub</p>
    </span>
  </div>
</template>

<script>
import axios from 'axios';

export default {
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
          if (error.response.status == "404") {
            this.notFound = true;
          }
          this.error = true;
        })
        .finally( () => this.loading = false )
      },
    },
    methods: {
      sort(repos) {
        return repos.slice().sort(function(a, b) {
          return b.stargazers_count - a.stargazers_count;
        });
      }
    }
    // computed: {
    //   sortedRepos() {
    //     return Object.keys(this.repos).sort((a, b) => { a.stargazers_count < b.stargazers_count ? -1 : 1 });
    //   }
    // }
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
