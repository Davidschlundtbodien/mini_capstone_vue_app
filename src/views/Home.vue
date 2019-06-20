<template>
  <div class="home">
    <h1>{{ message }}</h1>

    <ul>
      <li v-for="error in errors">{{error}}</li>
    </ul>

    
    <div class="form-group">
      <label for="title">Search by Name</label>
      <input type="text" class="form-control" id="title" placeholder="Enter product name..." v-model="nameFilter" list="names">
    </div> 

    <div class="form-group">
      <button class="btn btn-info" v-on:click="setSortAttribute('name')">Sort by name A-Z</button>
      <button class="btn btn-info" v-on:click="setSortAttribute('price')">Sort by price</button>
    </div><br>

    <datalist id="names">
      <option v-for="product in products">{{product.name}}</option>
    </datalist> 

    <div v-for="product in orderBy(filterBy(products, nameFilter), sortAttribute, sortAscending)">
      <h3>{{product.name}}</h3>

      <router-link v-bind:to="'/products/' + product.id">
      <img v-bind:src="product.image_url" alt="">
      </router-link>
      
      <div>
        <button v-on:click="showProduct(product)"> More Info</button>
      </div>

      <div v-if="product === currentProduct">
      </div> 

    </div>
  </div>
</template>

<style>
</style>

<script>
import Vue2Filters from "vue2-filters";
import axios from "axios";
export default {
  mixins: [Vue2Filters.mixin],

  data: function() {
    return {
      message: "Product Index",
      errors: [],
      products: [],
      currentProduct: {},
      nameFilter:"",
      sortAttribute: "",
      sortAscending: 1
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
    setSortAttribute: function(attribute) {
      if (this.sortAttribute === attribute) {
        this.sortAscending = this.sortAscending * -1;
      } else {
        this.sortAscending = 1;
      }
      this.sortAttribute = attribute;
    }
  }
};
</script>