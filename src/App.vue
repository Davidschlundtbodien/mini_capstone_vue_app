<template>
  <div id="app">
    <div id="nav" class="navbar navbar-expand-lg navbar-light bg-light">
      <router-link to="/">Home</router-link> |
      <router-link to="/products/new">New Product</router-link>
    </div>
    <div class="container">
      <router-view/>
    </div>    
  </div>
</template>