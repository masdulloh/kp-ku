<template>
    <div class="buycomplete">
        <h2>Orders</h2>
        <p>Pesanan </p>
    </div>
</template>

<script>
import db from '@/firebase/init'
import firebase from 'firebase'

export default {
    name: 'BuyComplete',
    data(){
        return{

        }
    },
    created(){

        let ref = db.collection('orders')
        //get current product
        ref.doc(this.$route.params.id).get()
        .then(doc => {
            if (!doc.exists) {
                // Jika produk yang mau di edit tidak ditemukan
                console.log('No such product id!');
            } else {
                console.log('Document data:', doc.data());
            }
        })
        .catch(err => {
            console.log('Error getting documents', err);
        })
    },
    mounted(){
        //munculkan user sekarang
        //console.log(firebase.auth().currentUser)
    }
}
</script>

<style>

</style>