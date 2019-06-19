<template>
  <div class="products-new">
    <h3>Create a new product.</h3>

    <ul>
      <li v-for="error in errors">{{error}}</li>
    </ul>

    <form v-on:submit.prevent="submit()">
      <div class="form-row">
        <div class="form-group col-md-6">
          <label for="name">Name</label>
          <input type="text" class="form-control" id="name" placeholder="Product name" v-model="newProductName">
        </div>
        <div class="form-group col-md-6">
          <label for="price">Price</label>
          <input type="number" class="form-control" id="price" placeholder="32.00" v-model="newProductPrice">
        </div>
      </div>
      <div class="form-group">
        <label for="description">Description</label>
        <input type="text" class="form-control" id="description" placeholder="Write a short summary about the product" v-model="newProductDescription">
      </div>
      <div class="form-group">
        <label for="imageUrl">Image</label>
        <input type="text" class="form-control" id="imageUrl" placeholder="Place an img url here" v-model="newProductImageUrl">
      </div>
      <button type="submit" class="btn btn-info">Create New Product</button>
    </form>

  </div>
</template>

<script>
import axios from "axios";
export default {

  data: function() {
    return {
      errors: [],
      newProductName: "",
      newProductDescription: "",
      newProductPrice: "",
      newProductImageUrl: ""
    };
  },
  created: function() {

  },
  methods: {
    submit: function() {
      var params = {
        name: this.newProductName,
        description: this.newProductDescription,
        price: this.newProductPrice,
        image_url: this.newProductImageUrl
      };

      axios.post("/api/products", params).then(response => {
        this.$router.push("/");
      }).catch(error => {
        this.errors = error.response.data.errors;
      });
    }
  }
};
</script>