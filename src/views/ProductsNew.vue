<template>
  <div class="new">
    <div class="container home">
      <form v-on:submit.prevent="createProduct()">
        <h1>Create New Product</h1>
        <ul>
          <li class="text-danger" v-for="error in errors">{{ error }}</li>
        </ul>
        <div class="form-group">
          <label>Title:</label>
          <input type="text" class="form-control" v-model="title" />
        </div>
        <div class="form-group">
          <label>Author:</label>
          <input type="text" class="form-control" v-model="author" />
        </div>
        <div class="form-group">
          <label>Description:</label>
          <input type="text" class="form-control" v-model="description" />
        </div>
        <div class="form-group">
          <label>Price:</label>
          <input type="text" class="form-control" v-model="price" />
        </div>
        <div class="form-group">
          <label>Image:</label>
          <input type="text" class="form-control" v-model="imageUrl" />
        </div>
        <div class="form-group">
          <label>Supplier:</label>
          <input type="text" class="form-control" v-model="supplier" />
        </div>
        <input type="submit" class="btn btn-primary" value="Submit" />
      </form>
    </div>
  </div>
</template>

<style>
.btn {
  display: block;
  width: 20%;
  margin: 0 auto;
}
/*input {
  display: block;
  margin: 0 auto;
  width: 60%;
}*/
</style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      title: "",
      author: "",
      description: "",
      price: "",
      imageUrl: "",
      supplier: "",
      errors: []
    };
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
          this.$router.push("/");
        })
        .catch(error => (this.errors = error.response.data.errors)); // not complete, need to display on front end
    }
  }
};
</script>
