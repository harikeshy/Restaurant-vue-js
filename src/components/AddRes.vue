<template>
<HeaderMenu />
<h1>Add Restaurant</h1>
<form class="add-restaurant">
    <input type="text" v-model="restaurant.name" placeholder="Enter Name">
    <input type="text" v-model="restaurant.address" placeholder="Enter Address">
    <input type="text" v-model="restaurant.contact" placeholder="Enter Contact">
    <button type="button" v-on:click="addRestaurant">Add Restaurant</button>
</form>
</template>

<script>
import axios from 'axios'
import HeaderMenu from "./HeaderMenu.vue";
export default {
    name: 'AddRes',
    components: {
        HeaderMenu
    },
    data() {
        return {
            restaurant: {
                name: '',
                address: '',
                contact: ''
            }
        }
    },
    methods: {
        async addRestaurant() {
            const result = await axios.post("http://localhost:3000/restaurant", {
                name: this.restaurant.name,
                address: this.restaurant.address,
                contact: this.restaurant.contact
            });
            if (result.status == 201) {
                this.$router.push({name: 'HomePage'});
            }
        }
    },
    mounted() {
        let user = localStorage.getItem("user-info");
        if (!user) {
            this.$router.push({
                name: 'SignUp'
            })
        }
    }
}
</script>
