<template>
    <div>
        <table class='table' v-if='repos && Object.keys(repos).length != 0'>
            <thead>
            <tr>
                <th scope='col'>Name</th>
                <th scope='col'>Stars</th>
            </tr>
            </thead>
            <tbody >
            <tr v-for='repo in sort(repos)' :key='repo.id'> 
                <th scope='row'>{{repo.name}}</th>
                <td>{{repo.stargazers_count}}</td>
            </tr>
            </tbody>
        </table>
        <tbody v-else-if='repos && Object.keys(repos).length === 0'>
                <tr>User doesn't have any repos.</tr>
        </tbody>
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
    .row {
        display:-ms-flexbox;
        display:flex;-ms-flex-wrap:wrap;
        flex-wrap:wrap;
        margin-right:-15px;
        margin-left:-15px
    }

    .table {
        width:100%;
        margin-bottom:1rem;
        color:#212529
    }
    
    .table td, .table th {
        padding:.75rem;
        vertical-align:top;
        border-top:1px solid #dee2e6
    }
    
    .table thead th {
        vertical-align:bottom;
        border-bottom:2px solid #dee2e6
    }
    
    .table tbody + tbody {
        border-top:2px solid #dee2e6
    }
</style>