<template>
{{ title }}
<table>
    <tr>
        <td>ID</td>
        <td>Имя</td>
        <td>Фамилия</td>
        <td>Возраст</td>
        <td>Компания</td>
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
        <td>{{ user.title }}</td>
        <td>
            <button
                @click="editUser(user)"
            >📝</button>
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
    <select
        v-model="company_id"
    >
        <option
            v-for="company in companies"
            :key="company.id"
            :value="company.id"
        >{{ company.title }}</option>
    </select>
    <button
        @click.prevent="addUser"
    >Создать пользователя</button>
    <button
        v-if="current"
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
        companies: [],
        current: null,
        firstname: '',
        secondname: '',
        age: null,
        company_id: null,
    }
},
mounted() {
    this.loadTitle();
    this.loadCompanies();
    
},
methods: {
    async loadTitle() {
        const result = await axios.get('http://mysql.be/index.php')
        this.title = result.data.title
        this.users = result.data.users
    },
    async loadCompanies() {
        const result = await axios.get('http://mysql.be/companies.php')
        this.companies = result.data.companies
    },
    async addUser() {
        await axios.post('http://mysql.be/index.php', {
            firstname: this.firstname,
            secondname: this.secondname,
            age: this.age,
            company_id: this.company_id
        })
        this.clearForm();
        this.loadTitle();
    },
    clearForm() {
        this.current = null;
        this.firstname = '';
        this.secondname = '';
        this.age = null;
        this.company_id = null;
    },
    editUser(user) {
        this.firstname = user.firstname;
        this.secondname = user.secondname;
        this.age = user.age;
        this.current = user.id;
        this.company_id = user.company_id;
    },
    async deleteUser(id) {
        if(confirm('Удалить?')) {
            await axios.delete('http://mysql.be/index.php?id=' + id);
            this.loadTitle();
        }
    },
    async updateUser() {
        await axios.put('http://mysql.be/index.php', {
            id: this.current,
            firstname: this.firstname,
            secondname: this.secondname,
            age: this.age,
            company_id: this.company_id
        });
        this.clearForm();
        this.loadTitle();
    }
}
}
</script>

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