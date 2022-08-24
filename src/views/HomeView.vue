<script>
import { assertExpressionStatement } from '@babel/types';
import axios from "axios"

export default {
  data: function () {
    return {
      message: "Welcome to the Pets app!",
      pets: [],
      currentPet: {},
      newPet: { name: "name", animal: "animal type", age: "age" },
      errors: []
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
        this.pets.push(response.data)
      }).catch(errors => {
        console.log(errors.response.data)
        this.errors = errors.response.data
        document.querySelector("#error").showModal()
      })
    },
    showEditPet: function (pet) {
      console.log("opening pet editor")
      this.currentPet = pet
      document.querySelector("#edit").showModal()
    },
    updatePet: function () {
      console.log("updating pet")
      axios.patch(`http://localhost:3000/pets/${this.currentPet.id}.json`, this.currentPet).then(response => {
        console.log(this.currentPet)
      }).catch(errors => {
        console.log(errors.response.data)
        this.errors = errors.response.data
        document.querySelector("#error").showModal()
      })
    },
    deletePet: function () {
      console.log("deleting pet")
      axios.delete(`http://localhost:3000/pets/${this.currentPet.id}.json`).then(response => {
        console.log(response.data)
        var index = this.pets.indexOf(this.currentPet)
        this.pets.splice(index, 1)
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
      <button v-on:click="showEditPet(pet)">edit pet</button>
      <hr />
    </div>
  </div>

  <dialog id="details">
    <form method="dialog">
      name: {{ currentPet.name }} <br />
      animal type: {{ currentPet.animal }} <br />
      age: {{ currentPet.age }} <br />
      <button>close</button>
    </form>
  </dialog>

  <dialog id="edit">
    <form method="dialog">
      <input type="text" v-model="currentPet.name"> <br />
      <input type="text" v-model="currentPet.animal"> <br />
      <input type="text" v-model="currentPet.age"> <br />
      <button v-on:click="updatePet">update</button>
      <button v-on:click="deletePet">delete</button>
      <button>close</button>
    </form>
  </dialog>

  <dialog id="error">
    <form method="dialog">
      <div v-for="error in errors.errors">
        {{ error }} <br />
      </div>
      <button>close</button>
    </form>
  </dialog>
</template>



<style>
</style>