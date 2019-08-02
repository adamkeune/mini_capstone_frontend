<template>
  <div class="container home">
    <h1>{{ message }}</h1>
    <div v-for="product in products">
      <div class="card">
        <h5 class="card-header">{{ product.title }}</h5>
        <div class="card-body">
          <h5 class="card-title">by {{ product.author }}</h5>
          <router-link v-bind:to="`/products/${product.id}`">
            More info
          </router-link>
        </div>
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

input {
  display: block;
  margin: 0 auto;
  width: 60%;
}

button {
  margin-top: 10px auto;
}
</style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      message: "Welcome to BookWyrm!",
      products: [],
      currentProduct: {},
      button_message: "More Info"
    };
  },
  created: function() {
    axios.get("/api/products").then(response => {
      this.products = response.data;
      console.log(this.products);
    });
  },
  methods: {
    showProduct(product) {
      if (this.currentProduct === product) {
        this.currentProduct = {};
        this.button_message = "More Info";
      } else {
        this.currentProduct = product;
        this.button_message = "Less Info";
      }
    },
    updateProduct(product) {
      let params = {
        title: product.title,
        author: product.author,
        description: product.description,
        price: parseInt(product.price)
      };
      axios.patch(`/api/products/${product.id}`, params).then(response => {
        console.log("Success!", response.data);
      });
    },
    destroyProduct(product) {
      axios.delete(`api/products/${product.id}`).then(response => {
        console.log("Product deleted!", response.data);
      });
      let index = this.products.indexOf(product);
      this.products.splice(index, 1);
    }
  }
};
</script>
