<template>
    <div class="container mt-5">
        <div class="card">
            <div class="card-header">
                <h4>product Lists</h4>
                <nuxt-link to="/product/create" class="btn btn-primary float-end">Add product</nuxt-link>
                <div class="card-body">

                    <div v-if="isLoading"> 
                        <Loading title="Loading..."/>
                    </div>
                    <div v-else>
                        <table class="table table-striped table-bordered">
                        <thead>
                            <th>ID</th>
                            <th>name</th>
                            <th>image</th>
                            <th>description</th>
                            <th>created at</th>
                            <th>action</th>
                        </thead>
                        <tbody>
                            <tr v-for="(product, index) in products" :key='index'>
                                <td>{{ product.id }}</td>
                                <td>{{ product.name }}</td>
                                <td><img :src="`http://127.0.0.1:8000/Storage/${product.image}`" style="width: 50%;" /></td>
                                <td>{{ product.description }}</td>
                                <td>{{ product.created_at }}</td>
                                <td><nuxt-link class="btn btn-success btn-xm mx-2" :to="`product/${product.id}`">Edit</nuxt-link>
                                    <button type="button" @click="deleteProduct($event,product.id)" class="btn btn-danger btn-sm mx-2">Delete</button>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                    </div>


                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
// import Loading from '~/components/Loading.vue';

            export default{
                // components: { Loading },
                name:"product",
                data(){
                    return{
                        products: {},
                        isLoading:true,
                    }
                },
                mounted(){
                    this.getProduct();
                },
                methods:{

                    getProduct(){
                        this.isLoading=true;
                        axios.get('http://127.0.0.1:8000/api/products').then(res => {
                            // console.log(res.data.products);
                            this.isLoading=false;
                            this.products = res.data.products;

                        })
                    },

                    deleteProduct(event, productId){
                        if(confirm('are you sure?'))
                        {
                            event.target.innerText='Deleting';
                            axios.delete(`http://127.0.0.1:8000/api/productdelete/${productId}`).then(res=>{
                            event.target.innerText='Delete';
                            this.getProduct();

                            });
                        }
                    }
                }
            }
</script>
<style lang="scss" scoped>
</style>