<template>
  <div class="home">
    <h1>{{ message }}</h1>

    <ul>
      <li v-for="error in errors">{{error}}</li>
    </ul>
    


    <div v-for="product in products">
      <h3>{{product.name}}</h3>

      <router-link v-bind:to="'/products/' + product.id">
      <img v-bind:src="product.image_url" alt="">
      </router-link>
      
      <div>
        <button v-on:click="showProduct(product)"> More Info</button>
      </div>

      <div v-if="product === currentProduct">
        <div>
          <button v-on:click="destroyProduct(product)">Delete</button>
        </div>
      </div> 

    </div>
  </div>
</template>

<style>
</style>

<script>
import axios from "axios";
export default {

  data: function() {
    return {
      message: "Product Index",
      errors: [],
      products: [],
      currentProduct: {},
    };
  },
  created: function() {
    axios.get("/api/products").then(response => {
      this.products = response.data;
    });
  },
  methods: {
    createProduct: function() {
      var params = {
        name: this.newProductName,
        description: this.newProductDescription,
        price: this.newProductPrice,
        image_url: this.newProductImageUrl
      };

      axios.post("/api/products", params).then(response => {
        console.log("product created", response.data);
        this.products.unshift(response.data);
        this.newProductName = "";
        this.newProductDescription = "";
        this.newProductPrice = "";
        this.newProductImageUrl = "";
      }).catch(error => {
        this.errors = error.response.data.errors;
      });
    },
    showProduct: function(product) {
      if (this.currentProduct === product) {
        this.currentProduct = null;
      } else {
        this.currentProduct = product;
      }
    },
    destroyProduct: function(product) {
      axios.delete("/api/products/" + product.id).then(response => {
        console.log("Product Removed", response.data);
        this.products.splice(this.products.indexOf(product),1);
      }).catch(error => {
        this.errors = error.response.data.errors;
      });
    }
  }
};
</script>