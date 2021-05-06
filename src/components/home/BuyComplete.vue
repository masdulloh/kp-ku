<template>
    <div class="buycomplete">
        <h2>Buy Complete</h2>
        <p class="text-center" v-if="orderdata">Pesanan {{ orderdata.gross }}</p>
        <p>Silahkan melakukan pembayaran sebesar</p>
        <p v-if="bankdata">pada nomor rekening dibawah ini {{ bankdata.account_number }}</p>
    </div>
</template>

<script>
import db from '@/firebase/init'
import firebase from 'firebase'
import axios from 'axios'

export default {
    name: 'BuyComplete',
    data(){
        return{
            orderdata:null,
            bankdata:null
        }
    },

    created(){
        //digunakan pihak ke 3 untuk request API raja ongkir karena jika menggunakan vue tidak dapat langsung request api
        axios.get('https://api.url.my.id/api/raja-ongkir/mootabank')
        .then(response => {
            // handle success
            this.bankdata= response.data.body[0];
        })
        .catch(function (error) {
            // handle error
            console.log(error);
        })
        .then(function () {
            // always executed
        });

        let ref = db.collection('orders')
        //get current product
        ref.doc(this.$route.params.id).get()
        .then(doc => {
            if (!doc.exists) {
                // Jika produk yang mau di edit tidak ditemukan
                console.log('No such order id!');
            } else {
                //console.log('Order data:', doc.data());
                this.orderdata=doc.data();
            }
        })
        .catch(err => {
            console.log('Error getting order documents', err);
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