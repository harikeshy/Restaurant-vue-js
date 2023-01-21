<template>
<HeaderMenu />
<h1>Update Restaurant</h1>
<form class="update-restaurant">
    <input type="text" v-model="restaurant.name" placeholder="Enter Name">
    <input type="text" v-model="restaurant.address" placeholder="Enter Address">
    <input type="text" v-model="restaurant.contact" placeholder="Enter Contact">
    <button type="button" v-on:click="updateRestaurant">Update Restaurant</button>
</form>
</template>

<script>
import axios from 'axios'
import HeaderMenu from "./HeaderMenu.vue";
export default {
    name: 'UpdateRes',
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
    methods:{
       async updateRestaurant()
        {
            const result = await axios.put("http://localhost:3000/restaurant/"+this.restaurant.id, {
                name: this.restaurant.name,
                address: this.restaurant.address,
                contact: this.restaurant.contact
            });
            if (result.status == 200) {
                this.$router.push({name: 'HomePage'});
            }
        }
    },
    async mounted() {
        let user = localStorage.getItem("user-info");
        if (!user) {
            this.$router.push({
                name: 'SignUp'
            })
        }
        //console.log(this.$route.params.id)
        const result = await axios.get("http://localhost:3000/restaurant/" + this.$route.params.id);
        this.restaurant = result.data
    }
}
</script>
