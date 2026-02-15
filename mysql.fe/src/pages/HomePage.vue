<script>
    import axios from 'axios';
    export default {
        data() {
            return {
                title: '',
                users: []
            }
        },
        methods: {
            async loadTitle() {
                const result = await axios.get('http://mysql.be/index.php')
                this.title = result.data.title
                this.users = result.data.users
            }
        }
    }
</script>

<template>
  {{ title }}
  <button
    @click="loadTitle"
  >Загрузить данные</button>
<table>
    <tr>
        <td>ID</td>
        <td>Имя</td>
        <td>Фамилия</td>
        <td>Возраст</td>
    </tr>
    <tr
        v-for="user in users"
        :key="user.id"
    >
        <td>{{ user.id }}</td>
        <td>{{ user.firstname }}</td>
        <td>{{ user.secondname }}</td>
        <td>{{ user.age }}</td>
    </tr>
</table>
</template>

<style>
    table, td {
        border: 1px solid black;
    }

    table {
        border-collapse: collapse;
    }

    td {
        padding: 5px 10px;
    }
</style>