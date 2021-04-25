<template>
    <div class="container">
        <h3>Repositories:</h3>
        <table class="table">
            <thead>
            <tr>
                <th scope="col">Name</th>
                <th scope="col">Stars</th>
            </tr>
            </thead>
            <tbody v-if="repos && Object.keys(repos).length != 0">
            <tr v-for="repo in sort(repos)" :key="repo.id"> 
                <th scope="row">{{repo.name}}</th>
                <td>{{repo.stargazers_count}}</td>
            </tr>
            </tbody>
            <tbody v-else-if="repos && Object.keys(repos).length === 0">
                <p>User doesn't have any repos.</p>
            </tbody>
        </table>
    </div>  
</template>

<script>
export default {
    props: ['repos'],
    methods: {
      sort(repos) {
        return repos.slice().sort(function(a, b) {
          return b.stargazers_count - a.stargazers_count;
        });
      }
    }
};
</script>