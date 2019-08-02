<template>
  <div class="container home">
    <div class="card">
      <h5 class="card-header">{{ product.title }}</h5>
      <div class="card-body">
        <h5 class="card-title">by {{ product.author }}</h5>
        <img :src="product.image_url" alt="" />
        <p class="card-text">{{ product.description }}</p>
        <h4 class="card-text">${{ product.price }}</h4>
        <hr />
        <router-link to="/" href="#">Back to Index</router-link>
        <router-link :to="`/products/${product.id}/edit`" href="#">
          Edit this Product
        </router-link>
      </div>
    </div>
  </div>
</template>

<style>
img {
  height: 300px;
  width: auto;
  display: block;
  margin: 10px auto;
}
</style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      product: {}
    };
  },
  created: function() {
    axios.get(`/api/products/${this.$route.params.id}`).then(response => {
      this.product = response.data;
      console.log(this.product);
    });
  },
  methods: {
    // destroyProduct(product) {
    //   axios.delete(`api/products/${product.id}`).then(response => {
    //     console.log("Product deleted!", response.data);
    //   });
    //   let index = this.products.indexOf(product);
    //   this.products.splice(index, 1);
    // }
  }
};
</script>
