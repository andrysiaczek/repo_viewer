<template>
    <div>
        <table class='table' v-if='repos && Object.keys(repos).length !== 0'>
            <thead>
            <tr>
                <th scope='col'>Name</th>
                <td scope='col'>Stars</td>
            </tr>
            </thead>
            <tbody>
            <tr v-for='repo in sort(repos)' :key='repo.id'> 
                <th scope='row'>{{ repo.name }}</th>
                <td>{{ repo.stargazers_count }}</td>
            </tr>
            </tbody>
        </table>
        <p class='empty' v-else-if='repos && Object.keys(repos).length === 0'>
                User doesn't have any public repositories.
        </p>
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

<style>
    tbody tr:hover {
        background-color: rgb(24, 25, 33);
        color: whitesmoke;
    }

    .empty {
        text-align: center;
        align-self: center;
    }
</style>