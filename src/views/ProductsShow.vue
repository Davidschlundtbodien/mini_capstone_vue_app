<template>
  <div class="products-show">
    <div class="container">
      <h1>{{product.name}}</h1>
      <img v-bind:src="product.image_url" alt="">
      <h4>Price: {{product.price}}</h4>
      <p>{{product.description}}</p>
    </div>

    <button><router-link v-bind:to="'/products/' + product.id + '/edit'">Edit</router-link></button>

  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      product: {}
    };
  },
  created: function() {
    axios.get("/api/products/" + this.$route.params.id).then(response => {
      this.product = response.data;
      console.log(this.product);
    });
  },
  methods: {
    
  }
};
</script>