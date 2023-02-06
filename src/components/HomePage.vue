<template>
<HeaderMenu />
<h1>Hello {{name}}, Welcome to Home page</h1>
<table border="1">
    <tr>
        <td>Id</td>
        <td>Name</td>
        <td>Address</td>
        <td>Contact</td>
        <td>Action</td>
    </tr>

    <tr v-for="item in restaurants" :key="item.id">
        <td>{{item.id}}</td>
        <td>{{item.name}}</td>
        <td>{{item.address}}</td>
        <td>{{item.contact}}</td>
        <td>
            <router-link :to="'/update/'+item.id">Edit</router-link>
            <a class="del_rec" href="#" v-on:click="deleteRestaurant(item.id)">Delete</a>
        </td>
    </tr>
</table>
</template>

<script>
import axios from 'axios'
import HeaderMenu from "./HeaderMenu.vue";
export default {
    name: 'HomePage',
      components: {
        HeaderMenu
    },
    data() {
        return {
            name: '',
            restaurants: []
        }
    },
  
    methods: {
        async deleteRestaurant(id) {
            if(confirm("Are you sure to delete record?")) {
            let result = await axios.delete("http://localhost:3000/restaurant/" + id);
            if (result.status == 200) {
                this.loadData()
            }
            }
        },
        async loadData() {

            let user = localStorage.getItem("user-info");
            this.name = JSON.parse(user).name // user data stored in json string format
            if (!user) {
                this.$router.push({
                    name: 'SignUp'
                })
            }

            let result = await axios.get("http://localhost:3000/restaurant");
            console.warn(result)
            this.restaurants = result.data
        }
    },
    mounted() {  // when page all loaded
        this.loadData()
    }
}
</script>

<style>
table {
    margin: 0 auto;
}

td {
    width: 160px;
    height: 40px;
}

.del_rec {
    padding-left: 10px;
}
</style>
