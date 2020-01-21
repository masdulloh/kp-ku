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
                    <td class="align-middle">{{ order.date.toDate() }}</td>
                    <td class="align-middle">Rp.{{ order.gross }}</td>
                    <td class="align-middle">
                        <div v-if="order.status==='Pending'" class="alert alert-warning" role="alert">{{ order.status }}</div>
                        <div v-else-if="order.status==='Processing'" class="alert alert-secondary" role="alert">{{ order.status }}</div>
                        <div v-else-if="order.status==='Completed'" class="alert alert-success" role="alert">{{ order.status }}</div>
                        <div v-else-if="order.status==='Cancelled'" class="alert alert-danger" role="alert">{{ order.status }}</div>
                    </td>
                    <td class="align-middle">
                        <div v-if="order.paystatus==='Unpaid'" class="alert alert-warning" role="alert">{{ order.paystatus }}</div>
                        <div v-else-if="order.paystatus==='Paid'" class="alert alert-success" role="alert">{{ order.paystatus }}</div>
                        <div v-else-if="order.paystatus==='Refunded'" class="alert alert-danger" role="alert">{{ order.paystatus }}</div>
                    </td>
                    <td class="align-middle">
                        <div class="btn-group">
                        <button class="btn btn-secondary btn-sm dropdown-toggle" type="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                            Action
                        </button>
                        <div class="dropdown-menu">
                            <a class="dropdown-item" v-if="order.status!=='Pending'" @click="statusnya('Pending', order.idorder)" exact :style="{ cursor: 'pointer'}">Pending</a>
                            <a class="dropdown-item" v-if="order.status!=='Processing'" @click="statusnya('Processing', order.idorder)" exact :style="{ cursor: 'pointer'}">Processing</a>
                            <a class="dropdown-item" v-if="order.status!=='Completed'" @click="statusnya('Completed', order.idorder)" exact :style="{ cursor: 'pointer'}">Completed</a>
                            <a class="dropdown-item" v-if="order.status!=='Cancelled'" @click="statusnya('Cancelled', order.idorder)" exact :style="{ cursor: 'pointer'}">Cancelled</a>
                            <div class="dropdown-divider"></div>
                            <a class="dropdown-item" v-if="order.paystatus!=='Unpaid'" @click="statusnya('Unpaid', order.idorder)" exact :style="{ cursor: 'pointer'}">Unpaid</a>
                            <a class="dropdown-item" v-if="order.paystatus!=='Paid'" @click="statusnya('Paid', order.idorder)" exact :style="{ cursor: 'pointer'}">Paid</a>
                            <a class="dropdown-item" v-if="order.paystatus!=='Refunded'" @click="statusnya('Refunded', order.idorder)" exact :style="{ cursor: 'pointer'}">Refunded</a>
                        </div>
                        </div>
                    </td>
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
        statusnya(isi, prod){
            db.collection('orders').doc(prod).update({status: isi})
        },
        paystatusnya(isi, prod){
            db.collection('orders').doc(prod).update({paystatus: isi})
        },

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