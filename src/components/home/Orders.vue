<template>
    <div class="orders">
        <h2>Orders</h2>

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
                <tr v-for="(order, index) in orderdisplay" :key="index">
                    <td class="align-middle"><img :src=product.pimage alt="" width="75"></td>
                    <td class="align-middle">{{ order.pname }}</td>
                    <td class="align-middle">{{ order.pprice }}</td>
                    <td class="align-middle">
                        <button @click="editProduct(order.idprod)" type="button" class="btn btn-success">Edit</button>
                        <button @click="deleteProduct(order.idprod, index)" type="button" class="btn btn-danger">Delete</button>
                        <a class="text-primary" @click="buyProduct(order.plink)" exact :style="{ cursor: 'pointer'}">checkout page</a>
                    </td>
                </tr>
                <!-- END -->

            </tbody>
        </table>
    </div>
</template>

<script>
import firebase from 'firebase'
export default {
    name: 'Orders',
    data(){
        return{
            orderdisplay: []
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