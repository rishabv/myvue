<script>
import axios from "axios";

export default {
  data() {
    return {
      drinks: [],
      isloading: false,
      message: "",
    };
  },
  async mounted() {
    this.isloading = true;
    let result = await axios.get(
      "https://www.thecocktaildb.com/api/json/v1/1/search.php?f=a"
    );
    this.isloading = false;
    console.log(result);
    if (result.status === 200) {
      this.drinks = result.data.drinks;
    } else {
      this.message = "404 not found";
    }
  },
};
</script>
<template>
  <b-container>
    <div>  
    <b-card-group deck class="mt-1 d-flex flex-wrap">
      <b-card
        v-for="item in drinks" :key="item.idDrink"
        v-bind:title="item.strIngredient1"
        v-bind:img-src="item.strDrinkThumb"
        img-alt="Image"
        img-top
        tag="article"
        style="max-width: 20rem"
        class="mb-2 mx-2"
      >
        <b-card-text>
          {{ item.strAlcoholic || "NA" }} | {{item.strCategory}}
        </b-card-text>

        <b-button href="#" variant="primary">Go somewhere</b-button>
      </b-card>
    </b-card-group>
    </div>
    <div class="my-2" v-if="this.isloading">
      <h1>Loading...</h1>
    </div>
  </b-container>
</template>