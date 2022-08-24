<script>
import { assertExpressionStatement } from '@babel/types';
import axios from "axios"

export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      pets: []
    };
  },
  created: function () {
    this.getPets()
  },
  methods: {
    getPets: function () {
      console.log("getting pets")
      axios.get("http://localhost:3000/pets.json").then(response => {
        this.pets = response.data
        console.log(this.pets)
      })
    }
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div v-for="pet in pets">
      {{ pet.name }} <br />
      <button v-on:click="showPet">more info</button>
      <hr />
    </div>
  </div>
</template>

<style>
</style>