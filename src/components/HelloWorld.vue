<script>
import axios from "axios";
import TableVue from "./TableVue.vue";

export default {
  props: {
    msg: String,
  },
  components: { TableVue },
  data() {
    return {
      form: {
        id: "",
        product_name: "",
        price: "",
      },
      name: "Todo List",
      products: [],
      fields: ["product_name", "price", "id", "actions"],
      query: "",
      posts: [],
    };
  },

  async mounted() {
    let result = await axios.get(
      `https://api.instantwebtools.net/v1/passenger?page=${0}&size=10`
    );
    this.posts = await result.data.data;
    console.log(result.data);
    let data = localStorage.getItem("products");
    if (data) {
      this.products = JSON.parse(data);
    }
  },
  methods: {
    add() {},
    onSubmit(e) {
      e.preventDefault();
      let { products } = this;
      if (products.some((i) => i.id == this.form.id)) {
        console.log(products, this.form.id);
        let index = this.products.findIndex((i) => i.id == this.form.id);
        this.products[index] = {
          product_name: this.form.product_name,
          price: this.form.price,
          id: this.form.id,
        };
        console.log(this.products, index);
      } else {
        this.form.id = Math.random().toString(9).slice(2, 7);
        this.products.push({
          product_name: this.form.product_name,
          price: this.form.price,
          id: this.form.id,
        });
      }
      this.store(this.products);
      this.form.product_name = "";
      this.form.price = "";
      this.form.id = "";
    },
    del(id) {
      console.log(id);
      this.products = this.products.filter((i) => i.id !== id);
      this.store(this.products);
    },
    edit(prdt) {
      this.form.id = prdt.id;
      this.form.product_name = prdt.product_name;
      this.form.price = prdt.price;
    },
    store(arr) {
      localStorage.setItem("products", JSON.stringify(arr));
    },
    onSearch() {
      console.log(this.query);
      let results = JSON.parse(localStorage.getItem("products"));
      if (this.query) {
        console.log(results);
        results = results.filter((i) => {
          return i.product_name.includes(this.query);
        });
      } else {
        // return;
      }
      console.log(results);
      this.products = results;
    },
  },
};
</script>
<template>
  <div class="container">
    <!-- <b-form @submit="onSubmit">
      <b-form-group
        id="input-group-2"
        label="Product Name:"
        label-for="input-2"
      >
        <b-form-input
          id="input-2"
          v-model="form.product_name"
          placeholder="Enter product name"
          required
        ></b-form-input>
      </b-form-group>

      <b-form-group
        id="input-group-2"
        label="Product price:"
        label-for="input-2"
      >
        <b-form-input
          id="input-2"
          pattern="^[1-9]\d*$"
          title="Only numbers are allowed"
          v-model="form.price"
          placeholder="Enter price"
          required
        ></b-form-input>
      </b-form-group>

      <b-button type="submit" variant="primary" class="my-3">Submit</b-button>
    </b-form>
    <b-card class="mt-3" header="Form Data Result">
      <b-form-input
        id="inline-form-input-name"
        class="mb-2 mr-sm-2 mb-sm-0"
        placeholder="Search here..."
        v-on:input="onSearch"
        v-model="query"
      ></b-form-input>

      <b-table
        :fields="this.fields"
        :items="products"
        v-if="products.length > 0"
      >
        <template #cell(actions)="row">
          <b-button variant="outline-primary" size="md" @click="edit(row.item)"
            >edit</b-button
          >
          <b-button
            variant="outline-danger"
            class="mx-2"
            size="md"
            @click="del(row.item.id)"
            >delete</b-button
          >
        </template>
      </b-table>
    </b-card> -->
    <TableVue/>
  </div>
</template>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.container {
  width: 50% !important;
}
</style>
