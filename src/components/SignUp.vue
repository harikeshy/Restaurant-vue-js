<template>
<img class="logo" src="../assets/logo.jpg">
<h1>Sign Up</h1>
<div class="register">
<input type="text" v-model="name" placeholder="Enter name">
<input type="text" v-model="email" placeholder="Enter email">
<input type="password" v-model="password" placeholder="Enter password">
<button v-on:click="signUp">Sign Up</button>
</div>
<p>
<router-link to="/login">Login</router-link>
</p>
</template>

<script>
import axios from 'axios'
export default {
    name: 'SignUp',    // component name, we can use any name but its recomended
    data()
    {
        return {
            name:'',
            email:'',
            password:''
        }
    },
    methods:{
        async signUp()  // we use async and wait because some time api get late response
        {
            console.warn("signup", this.name,this.email,this.password)
            let result = await axios.post("http://localhost:3000/users",{
                "name":this.name,
                "email":this.email,
                "password":this.password
            });
            console.warn(result);
            if(result.status==201)    // api shows status 201 when record save
            {
                
                localStorage.setItem("user-info",JSON.stringify(result.data))   // we store all data in local storage to check user alreay login
                this.$router.push({name:'HomePage'})   // redirect to page
            }
            
        }
    },
    mounted()
    {
        let user = localStorage.getItem("user-info");
        if(user){
            this.$router.push({name:"HomePage"})
        }
    }
}
</script>
