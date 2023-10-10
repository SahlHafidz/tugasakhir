<template>
    <div class="mt-5 container">
    <h3>heheheh</h3>
    <nuxt-link to="../index">product</nuxt-link>
    
    <div class="card">
        <div class="card-header">
            <h4>add product
                <nuxt-link to='/' class="btn btn-danger float-end">Back</nuxt-link>
            </h4>
        </div>
        <div class="card-body">
            
            <div v-if="isLoading">
                <Loading :title="isLoadingTitle"/>
            </div>

            <form @submit.prevent="saveProduct">
                <div class="mb-3">
                    <label for="">Name</label>
                    <input type="text" v-model="product.name" class="form-control"  name="" id="">
                    <span class="text-danger" v-if="this.errorList.name">{{ this.errorList.name[0] }}</span>
                </div>
                <div class="mb-3">
                    <label for="">image</label>
                    <input type="file" class="form-control" @change="product.image" name="" id="">
                    <span class="text-danger" v-if="this.errorList.image">{{ this.errorList.image[0] }}</span>
                </div>

                <div class="mb-3">
                    <label for="">Description</label>
                    <input type="text" class="form-control" v-model="product.description" name="" id="">
                    <span class="text-danger" v-if="this.errorList.description">{{ this.errorList.description[0] }}</span>
                </div>
                <div class="mb-3">
                    <button type="submit" class="btn btn-primary">Save</button>
                </div>
            </form>
        </div>
    </div>
    

</div>


</template>
<script>
import axios from'axios';
export default{
    name:"ProductCreate",
    data(){
        return{
            product:{
                name:'',
                image:null,
                description:'',

            },
            isLoading:false,    
            isLoadingTitle:'Loading',
            errorList:{}
        }
    },
    methods:{
        handleFileChange(event) {
                    // Mengambil file yang dipilih dari input
                    const file = event.target.files[0];

                    // Menyimpan file ke dalam variabel selectedFile
                    this.product.image = file;
                },
        saveProduct()
            {
            this.isLoading=true;
            this.isLoadingTitle="Saving";
            alert('am here')
            const myThis=this;
            // this.handleFileChange();
            axios.post('http://127.0.0.1:8000/api/products',this.product).then(res=>{
                console.log(res,'res');
                alert(res.data.message);

                this.product.name='';
                // this.handleFileChange()
                this.product.image=null;
                this.product.description='';

                this.isLoading=false;
                this.isLoadingTitle="Loading";
            })
            .catch(function (error){
                if(error.response){
                    console.log(error,'errors')
                    if(error.response.status === 422){

                        myThis.errorList = error.response.data.errors;
                        this.isLoading=false;
                    }
                }
                
            });
        }
    }
}
</script>