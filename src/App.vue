<template>
  <div id="app">
    <div id="nav" class="navbar navbar-expand-lg navbar-light bg-light">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>|
      <router-link to="/sample">Get dem SAMPS!</router-link>|
      <router-link to="/products/new">New Product</router-link>
    </div>
    <div class="container">
      <router-view/>
    </div>    
  </div>
</template>