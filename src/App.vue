<template>
  <div class="column is-half is-offset-one-quarter">
    <h4 class="is-size-4">Pokedex</h4>
    <div class="field has-addons">
      <div class="control is-expanded">
        <input class="input" type="text" placeholder="Buscar pokemon pelo nome" v-model="search">
      </div>
      <div class="control">
        <a class="button is-success">
          Buscar
        </a>
      </div>
    </div>

    <!--<div v-for="(poke, index) in pokemons" :key="index">-->
    <div v-for="(poke, index) in searchResult" :key="index">
      <Pokemon :name="poke.name" :url="poke.url" :num="index + 1" />
    </div>
  </div>
</template>

<script>
import axios from "axios"
import Pokemon from "./components/Pokemon.vue"

export default {
  name: 'App',
  components: {
    Pokemon
  },
  data() {
    return {
      pokemons: [],
      search: ''
    }
  },
  created: function() { // executado sempre que uma página é caregada
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0").then(res => {
      this.pokemons = res.data.results
    }).catch(err =>{
      console.log(err)
    })
  },
  computed: {
    searchResult: function() {
      if(this.search.trim() == '') {
        return this.pokemons
      } else {
        return this.pokemons.filter(pokemon => pokemon.name.toLowerCase().includes(this.search.toLowerCase())) // includes is ECMAScript 5
      }
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.foto {
  width: 10%;
}
</style>
