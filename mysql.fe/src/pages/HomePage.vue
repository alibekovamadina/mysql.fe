<template>
{{ title }}
<table>
    <tr>
        <td>ID</td>
        <td>Имя</td>
        <td>Фамилия</td>
        <td>Возраст</td>
        <td></td>
    </tr>
    <tr
        v-for="user in users"
        :key="user.id"
    >
        <td>{{ user.id }}</td>
        <td>{{ user.firstname }}</td>
        <td>{{ user.secondname }}</td>
        <td>{{ user.age }}</td>
        <td>
            <button
                @click="editUser(user)"                                                               
            >✏</button>
            <button
                @click="deleteUser(user.id)"
            >❌</button>
        </td>
    </tr>
</table>
<form>
    <input
        type="text"
        placeholder="Имя"
        v-model="firstname"
    >
    <input
        type="text"
        placeholder="Фамилия"
        v-model="secondname"
    >
    <input
        type="number"
        placeholder="Возраст"
        v-model="age"
    >
    <button
        @click.prevent="addUser"
    >Создать пользователя</button>
    <button
        v-if="currentId"
        @click.prevent="updateUser"
    >Обновить пользователя</button>
</form>
</template>

<script>
import axios from 'axios';
export default {
data() {
    return {
        title: '',
        users: [],
        currentId: null,
        firstname: '',
        secondname: '',
        age: null
    }
},
mounted() {  //чтобы при загрузке сразу открылась таблица с данными//
    this.loadTitle();
},
methods: {
    async loadTitle() {
        const result = await axios.get('http://mysql.be/index.php')
        this.title = result.data.title
        this.users = result.data.users
    },
    async addUser() {
        await axios.post('http://mysql.be/index.php', {
            firstname: this.firstname,
            secondname: this.secondname,
            age: this.age
        })
        this.firstname = '';
        this.secondname = '';
        this.age = null;
        this.loadTitle();
    },
    editUser(user) {
        this.firstname = user.firstname;
        this.secondname = user.secondname;
        this.age = user.age;
        this.currentId = user.id;
    },
    async deleteUser(id) {
        if(confirm('Удалить')) {
            await axios.delete('http://mysql.be/index.php?id=' + id);
            this.loadTitle(); //обновление,повторная загрузка//
        }
    },
    async updateUser() {
        await axios.put('http://mysql.be/index.php', {
            id: this.currentId,
            firstname: this.firstname,
            secondname: this.secondname,
            age: this.age
        });
        this.loadTitle();
    }
}
}
</script>

<style>
    table, td {
        border: 1px solid black;
        color: white;
    }

    table {
        border-collapse: collapse;
        background-color: rgba(145, 4, 4, 0.959);
    }
    
    td {
        padding: 5px 10px;
    }
</style>