<template>
  <div>
      <div class="container">
             <div class="row">
              <div class="col-md-4 mb-2" v-for="(product,index) in products" :key=index >
                <div class="card text-center" style="width: 18rem;" >
                   <div class="card-body">
                  <img :src="product.image" class="card-img-top img">
                       <h3 class="mt-2">{{product.title}}</h3>
                    <div class="my-4 text-muted">{{product.price}}</div>
                    <div class="d-grid gap-2">
                      <router-link :to="`/product/${product.id}`" class="btn btn-primary" type="button">Button</router-link>
                     
                    </div>
                  </div>
                </div>
              
              </div>
             </div>
       
      </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data(){
    return {
      products: ""
    }

  },
    
methods: {
  get(){
     axios.get("https://fakestoreapi.com/products").then(response =>{ 
       this.products= response.data;
     })
  }
},
created(){
  this.get();
}
}
</script>

<style>
.img{
  height: 150px;
  object-fit: contain;
}
</style>