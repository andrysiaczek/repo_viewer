<template>
  <div class='container'>
    <div class='header'>
      <h1>RepoViewer</h1>
      <h5>See what others coded so far</h5>
    </div>
    <form class='form'>
      <label for='username'>Enter username: </label>
      <input id='username' v-model='username' type='text' name='username'>
      <input type='submit' @click='processForm' value='View'>
    </form>
    <div class='content'>
      <Repositories v-if='!loading && !notFound' :repos='repos'/>
      <span v-if='!loading && notFound'>
        <p>This user doesn't exist on GitHub</p>
      </span>
      <span v-if='error && !notFound'>
        <p>Some error occured. Try again later.</p>
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
        loading: true,
        error: false,
        notFound: false,
        request: true,
      }
    },
    methods: {
      processForm(e) {
        e.preventDefault();

        this.loading = true;
        if (this.username.length != 0 && this.request){
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
      }
    }
}
</script>

<style>
  .container {
    display: flex;
    flex-direction: column;
    width: 100%;
    background-color: whitesmoke;
    min-height: 100%;
  }

  .header {
    padding: 60px;
    background-color: rgb(24, 25, 33);
    color: rgb(80, 210, 214);
    width: 100%;
    align-self: flex-start;
  }

  .form {
    background-color: rgb(107, 220, 224);
    color: rgb(24, 25, 33);
    width: 100%;
    padding: 20px;
    display: flex;
    flex-direction: column;
  }

  input[type=text] {
    background-color: rgb(24, 25, 33);
    color: white;
    text-align: center;
  }

  .content {
    width: 100%;
    padding: 20px;
  }

  input[type=submit] {
    background-color: rgb(24, 25, 33);
    color: rgb(80, 210, 214);
  }
</style>