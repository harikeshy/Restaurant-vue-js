<template>
<img class="logo" src="../assets/logo.jpg">
<h1>Login</h1>
<div class="login">
    <input type="text" v-model="email" placeholder="Enter email">
    <input type="password" v-model="password" placeholder="Enter password">
    <button v-on:click="login">Login</button>
</div>
<p>
    <router-link to="/signup">Sign-Up</router-link>
</p>
</template>

<script>
import axios from 'axios'
export default {
    name: 'LoginPage',
    data() {
        return {
            email: '',
            password: ''
        }
    },
    methods: {
        async login() {
            let result = await axios.get(
                `http://localhost:3000/users?email=${this.email}&password=${this.password}`
            );
            if (result.status == 200 && result.data.length > 0) // api shows status 201 when record save
            {
                localStorage.setItem("user-info", JSON.stringify(result.data[0])) // we store all data in local storage to check user alreay login
                this.$router.push({
                    name: 'HomePage'
                }) // redirect to page
            }

        }
    },
    mounted() {
        let user = localStorage.getItem("user-info");
        if (user) {
            this.$router.push({
                name: "HomePage"
            })
        }
    }
}
</script>
