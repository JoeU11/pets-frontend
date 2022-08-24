<script>
import { assertExpressionStatement } from '@babel/types';
import axios from "axios"

export default {
  data: function () {
    return {
      message: "Welcome to the Pets app!",
      pets: [],
      currentPet: {},
      newPet: { name: "name", animal: "animal type", age: "age" }
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
    },
    showPet: function (pet) {
      console.log("showing pet")
      this.currentPet = pet
      document.querySelector("#details").showModal()
    },
    postPet: function () {
      console.log("creating pet")
      axios.post("http://localhost:3000/pets.json", this.newPet).then(response => {
        this.pets.push(this.newPet)
      })
    }
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <hr />
    <input type="text" v-model="newPet.name"> <br />
    <input type="text" v-model="newPet.animal"> <br />
    <input type="text" v-model="newPet.age"> <br />
    <button v-on:click="postPet">add pet</button>
    <hr />
    <div v-for="pet in pets">
      {{ pet.name }} <br />
      <button v-on:click="showPet(pet)">more info</button>
      <hr />
    </div>
  </div>

  <dialog id="details">
    <form model="dialog">
      name: {{ currentPet.name }} <br />
      animal type: {{ currentPet.animal }} <br />
      age: {{ currentPet.age }} <br />
      <button>close</button>
    </form>
  </dialog>
</template>



<style>
</style>