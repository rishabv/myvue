<script>
import axios from "axios";
export default {
  props: {
    msg: String,
  },
  data() {
    return {
      fields: ["name", "trips", "_id", "actions"],
      query: "",
      posts: [],
      isloading: false,
      currentPage: 1,
      totalPassengers:0,
      

    };
  },

  async mounted() {
    this.fetchData(1);
  },
  watch: {
    currentPage: function (val) {
      this.fetchData(val);
    },
  },
  methods: {
    del(id) {
      console.log(id);
    },
    async fetchData(page) {
      console.log("hi");
      this.isloading = true;
      let result = await axios.get(
        `https://api.instantwebtools.net/v1/passenger?page=${page}&size=10`
      );
      this.isloading = false;

      this.posts = result.data.data;
      this.totalPages = result.data.totalPages;
      this.totalPassengers = result.data.totalPassengers;
    },
    edit(prdt) {
      this.form.id = prdt.id;
      this.form.product_name = prdt.product_name;
      this.form.price = prdt.price;
    },
    onSearch() {
      console.log(this.query);
    },
  },
};
</script>
<template>
  <b-card class="mt-3" header="Form Data Result">
    <b-form-input
      id="inline-form-input-name"
      class="mb-2 mr-sm-2 mb-sm-0"
      placeholder="Search here..."
      v-on:input="onSearch"
      v-model="query"
    ></b-form-input>
    <p v-if="this.isloading">Loading ...please wait</p>
    <b-table :fields="this.fields" :items="posts" v-if="posts.length > 0">
      <template #cell(actions)="row">
        <b-button variant="outline-primary" size="md" @click="edit(row.item)"
          >open</b-button
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
    <b-pagination
      v-model="currentPage"
      :total-rows="totalPassengers"
      :per-page="10"
    ></b-pagination>
  </b-card>
</template>
