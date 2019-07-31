<template>
  <div class="container home">
    <h1>{{ message }}</h1>
    Title:
    <input v-model="title" type="text" />
    Author:
    <input v-model="author" type="text" />
    Description:
    <input v-model="description" type="text" />
    Price:
    <input v-model="price" type="text" />
    Image Url:
    <input v-model="imageUrl" type="text" />
    Supplier:
    <input v-model="supplier" type="text" />
    <button v-on:click="createProduct()">Create Product</button>
    <div v-for="product in products">
      <div class="card">
        <h5 class="card-header">{{ product.title }}</h5>
        <div class="card-body">
          <h5 class="card-title">by {{ product.author }}</h5>
          <button v-on:click="showProduct(product)" class="btn btn-primary">
            {{ button_message }}
          </button>
          <div v-if="product === currentProduct">
            <img :src="product.image_url" alt="" />
            <p class="card-text">{{ product.description }}</p>
            <h4 class="card-text">${{ product.price }}</h4>
            <hr />

            <h6>Edit Product</h6>
            <div>
              Title:
              <input v-model="product.title" type="text" />
              Author:
              <input v-model="product.author" type="text" />
              Description:
              <input v-model="product.description" type="text" />
              Price:
              <input v-model="product.price" type="text" />
              <button v-on:click="updateProduct(product)">Update</button>
            </div>
            <button v-on:click="destroyProduct(product)">Delete</button>
          </div>
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
      button_message: "More Info",
      title: "",
      author: "",
      description: "",
      price: "",
      imageUrl: "",
      supplier: ""
    };
  },
  created: function() {
    axios.get("/api/products").then(response => {
      this.products = response.data;
      console.log(this.products);
    });
  },
  methods: {
    createProduct: function() {
      console.log("Creating product...");
      var params = {
        title: this.title,
        author: this.author,
        image_url: this.imageUrl,
        description: this.description,
        price: this.price,
        supplier: this.supplier
      };
      axios
        .post("/api/products", params)
        .then(response => {
          console.log("Success!", response.data);
          this.title = "";
          this.author = "";
          this.description = "";
          this.price = "";
          this.imageUrl = "";
          this.supplier = "";
          this.products.push(response.data);
        })
        .catch(error => console.log(error.response)); // not complete, need to display on front end
    },
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
