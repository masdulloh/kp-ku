<template>
    <div class="orders">
        <h2>Orders</h2>
    </div>
</template>

<script>
import firebase from 'firebase'
export default {
    name: 'Orders',
    data(){
        return{

        }
    },
    mounted(){
        //munculkan user sekarang
        //console.log(firebase.auth().currentUser)
    }
}
</script>

<style>

</style>