<template>
<section id="contact" class="contact">
      <div class="container" data-aos="fade-up">

        <div class="section-title">
          <h2>Update Product</h2>
        <hr />
        </div>
    <div class="hero">

        <form style="margin:20px 50px" v-on:submit="updateProduct">
            <label>Name</label>
            <input type="text"  v-model="name"  />
            <label>Description</label>
            <textarea rows="6"  v-model="description" ></textarea>
            <label>Price</label>                    
            <input type="number"  v-model="price"  />
            <label>Quantity</label>
             <input type="number" v-model="quantity" />
            <button type="submit">Update</button>
        </form>
    </div>
      </div>
    </section>
</template>

<script>
import Toasted from "vue-toasted";

import VueProgressBar from "vue-progressbar";
export default {
  data() {
    return {
      name: "",
      description: "",
      price: "",
      quantity: "",
      product: {},
    };
  },

  methods: {
    async getProduct() {
      // this.$progress.start();
      const id = this.$route.params.id;
      const token = localStorage.getItem('token');
      const res = await fetch(
        `http://product-mgt-api.herokuapp.com/api/product/${id}`,
        {
          method: "GET",
          headers: {
            "Content-Type": "application/json",
            "Authorization": "Bearer " + token
          },
        }
      );
      const data = await res.json();
      if (res.status === 200) {
        console.log("data ", data);
        this.product = data;
        this.name = data.name;
        this.description = data.description;
        this.price = data.price;
        this.quantity = data.quantity;
      }
    },

    async updateProduct(e) {
       this.$Progress.start();
      e.preventDefault();
      const token = localStorage.getItem("token");
      const id = this.$route.params.id;
      const res = await fetch(
        `http://product-mgt-api.herokuapp.com/api/product/${id}`,
        {
          method: "PUT",
          headers: {
            "Content-Type": "application/json",
            "Authorization": "Bearer " + token
          },
          body: JSON.stringify({
            name: this.name,
            description: this.description,
            price: this.price,
            quantity: this.quantity,
          }),
        }
      );

      if (res.status === 200) {
        this.$toasted.show("Data updated");
        this.$router.push({ name: "list", params: { id: id } });
      }else{
        this.$Progress.fail(),
        this.$toasted.show(error.res.message);
      }
    },
  },

  mounted() {
    this.getProduct();
  },
};
</script>