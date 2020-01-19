<template>
    <div class="product">
        <h2>Product</h2>
        <router-link :to="{ name:'AddProduct' }"><button type="button" class="btn btn-success">+Add Product</button></router-link>
        <p>&nbsp;</p>


        <table class="table table-striped table-hover">
            <thead>
                <tr>
                <th scope="col">Image</th>
                <th scope="col">Product Name</th>
                <th scope="col">Price</th>
                <th scope="col">Action</th>
                </tr>
            </thead>
            <tbody>

                <!-- Perulangan menampilkan product dari database -->
                <tr v-for="(product, index) in productdisplay" :key="index">
                    <td class="align-middle"><img :src=product.pimage alt="" width="75"></td>
                    <td class="align-middle">{{ product.pname }}</td>
                    <td class="align-middle">{{ product.pprice }}</td>
                    <td class="align-middle">
                        <button @click="editProduct(product.idprod)" type="button" class="btn btn-success">Edit</button>
                        <button @click="deleteProduct(product.idprod, index)" type="button" class="btn btn-danger">Delete</button>
                        <a class="text-primary" @click="buyProduct(product.plink)" exact :style="{ cursor: 'pointer'}">checkout page</a>
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
    name: 'Products',
    data(){
        return{
            pname: null,
            productdisplay:[],
            pprice: null,
            plink: null,
            idprod: null
        }
    },
    created(){
        //view product
        this.showData()
        //console.log(this.productdisplay)
    },
    methods:{
        showData(){
            let productdisplay=[]
            db.collection('products')
            .onSnapshot((snapshot) => {
                snapshot.docChanges().forEach(change => {
                    if(change.type === 'added'){
                        this.productdisplay.unshift({
                            pname: change.doc.data().pname,
                            pprice: change.doc.data().pprice,
                            pimage: change.doc.data().pimage,
                            plink: change.doc.data().plink,
                            idprod: change.doc.id
                        })
                    }
                    if (change.type === 'removed') {
                        this.$router.push({ name:'Beranda' })
                    }
                })
            })
        },
        editProduct(id){
            //TODO
            this.$router.push({ name: 'EditProduct', params: {id: id } })
        },

        deleteProduct(doc,index){
            //productdisplay= this.productdisplay
            if (confirm('Are you sure ?')) {
                console.log(doc)
                db.collection('products').doc(doc).delete()
                .then(function() {
                    //console.log("berhasil di delete", index)
                    //this.productdisplay.splice(1,1)
                    //this.$delete(this.productdisplay, index)
                    //const filteredItems = this.productdisplay.slice(0, index).concat(this.productdisplay.slice(index + 1, this.productdisplay.length))
                }).catch(function(error){
                    console.log(error)
                })
            }
        },

        buyProduct(id){
            this.$router.push({ name:'Buy' , params: {id:id} })
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