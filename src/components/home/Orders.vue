<template>
    <div class="orders">
        <h2>Orders</h2>

        <table class="table table-striped table-hover">
            <thead>
                <tr>
                    <th scope="col">ID</th>
                    <th scope="col">Name</th>
                    <th scope="col">Date</th>
                    <th scope="col">Gross Revenue</th>
                    <th scope="col">Status</th>
                    <th scope="col">Payment</th>
                    <th scope="col">Action</th>
                </tr>
            </thead>
            <tbody>

                <!-- Perulangan menampilkan product dari database -->
                <tr v-for="(order, index) in orderdisplay" :key="index">
                    <td class="align-middle">{{ order.verif }}</td>
                    <td class="align-middle">{{ order.name }}<br>{{ order.whatsapp }}</td>
                    <td class="align-middle">
                        <button @click="editProduct(order.idorder)" type="button" class="btn btn-success">Edit</button>
                        <button @click="deleteProduct(order.idorder, index)" type="button" class="btn btn-danger">Delete</button>
                        <a class="text-primary" @click="buyProduct(order.idorder)" exact :style="{ cursor: 'pointer'}">checkout page</a>
                    </td>
                </tr>
                <!-- END -->

            </tbody>
        </table>
    </div>
</template>

<script>
import db from '@/firebase/init'
import firebase from 'firebase'

export default {
    name: 'Orders',
    data(){
        return{
            orderdisplay: []
        }
    },
    created(){
        //view product
        this.showData()
        //console.log(this.productdisplay)
    },
    methods:{
        // FUNGSI TAMPILKAN DATA
        showData(){
            let productdisplay=[]
            db.collection('orders')
            .onSnapshot((snapshot) => {
                snapshot.docChanges().forEach(change => {
                    if(change.type === 'added'){
                        this.orderdisplay.unshift({
                            verif: change.doc.data().verif,
                            name: change.doc.data().name,
                            whatsapp: change.doc.data().whatsapp,
                            date: change.doc.data().date,
                            gross: change.doc.data().gross,
                            status: change.doc.data().status,
                            paystatus: change.doc.data().paystatus,
                            idorder: change.doc.id
                        })
                    }
                    if (change.type === 'removed') {
                        this.$router.push({ name:'Beranda' })
                    }
                })
            })
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