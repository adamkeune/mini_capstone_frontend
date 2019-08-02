<template>
  <div class="container home">
    <h1>Edit Product</h1>
    <div>
      <img :src="product.image_url" alt="" />
      <p>Title:</p>
      <input v-model="product.title" type="text" />
      <p>Author:</p>
      <input v-model="product.author" type="text" />
      <p>Description:</p>
      <input v-model="product.description" type="text" />
      <p>Price:</p>
      <input v-model="product.price" type="text" />
      <button v-on:click="updateProduct(product)">Update</button>
      <button v-on:click="destroyProduct(product)">Delete</button>
    </div>
  </div>
</template>

<style>
h1 {
  text-align: center;
}

input {
  display: block;
  margin: 0 auto;
  width: 90%;
}

button {
  display: block;
  margin: 10px auto;
  width: 10%;
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
    updateProduct(product) {
      let params = {
        title: product.title,
        author: product.author,
        description: product.description,
        price: parseInt(product.price)
      };
      axios.patch(`/api/products/${product.id}`, params).then(response => {
        this.$router.push("/");
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
