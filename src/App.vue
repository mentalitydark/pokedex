<template>
  <div id="app">
    <img src="./assets/pokedex_logo.png" alt="Pokédex Logo">
    <input v-model="search" type="text" name="Search" id="Search" placeholder="Buscar Pokemon">
    <!-- <button @click="buscar" id="button">Buscar</button> -->
    <div class="columns is-4 is-justify-content-center is-flex is-flex-direction-row is-flex-wrap-wrap">
      <div v-for="(pokemon, index) in ResultSearch" :key="pokemon.url" :class="{'column is-4': ResultSearch.length > 1, 'column is-10' :  ResultSearch == 1} ">
        <Pokemon :nome="pokemon.name" :url="pokemon.url" :number="index+1"/>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import Pokemon from './components/Pokemon';
export default {
  name: 'App',
  data() {
    return {
      pokemons: [],
      filteredPokemons: [],
      search: ''
    }
  },
  components: {
    Pokemon
  },
  created: function() {
    axios.get('https://pokeapi.co/api/v2/pokemon?limit=151&offset=0').then(res => {
      console.log("Requisição Feita com sucesso.");
      this.pokemons = (res.data.results);
      this.filteredPokemons = res.data.results;
    }).catch( erro => {
      console.log("Erro ao fazer a requisição");
      console.log(erro);
    })
  },
  // methods: {
  //   buscar: function() {
  //     this.filteredPokemons = this.pokemons;
  //     if(this.search.trim() == '') {
  //       this.filteredPokemons = this.pokemons;
  //     } else {
  //       this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name.indexOf(this.search.toLowerCase()) >= 0)
  //     } 
  //   }
  // },
  computed: {
    ResultSearch: function() {
      if(this.search.trim() == '') {
        return this.pokemons;
      } else {
        return this.pokemons.filter(pokemon => pokemon.name.indexOf(this.search.toLowerCase()) >= 0)
      } 
    }
}
}
</script>

<style>
body {
  margin: 0;
  padding: 0;
  border: 0;
  /* min-width: 760px; */
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 30px;
  display: flex;
  flex-direction: column;
  flex-wrap: wrap;
  align-items: center;
}
#Search {
  /* position: absolute;
  top: 10px;
  left: 42vw; */
  text-align: center;
  font-size: 1.2em;
  border: 1px solid rgba(0, 0, 0, 0.1);
  outline: transparent;
  border-radius: 50px;
  padding: 10px 15px;
  width: 30%;
  margin-bottom: 20px;
  transition-duration: 0.5s;
  font-weight: 500;
}
#Search:focus {
  border: 1px solid rgb(105, 105, 196);
}
#Search:focus::placeholder {
  color: rgb(105,105,196)
}
#Search::placeholder{
  color: rgba(0, 0, 0, 0.3);
  transition-duration: 0.5s;
}
#app > img {
  margin-bottom: 20px;
  width: 300px;
}
#button {
  border: 2px solid rgba(0, 0, 0, 0.1);
  outline: transparent;
  width: 20%;
  padding: 10px 5px;
  border-radius: 10px;
  background: white;
  font-size: 1em;
  font-weight: 500;
  cursor: pointer;
  transition-duration: 0.5s;
}
#button:hover {
  border: 2px solid rgb(118, 129, 228);
  background-color: rgb(233, 233, 99);
}
html::-webkit-scrollbar {
  display: none;
}
html {
  -ms-overflow-style: none;
  scrollbar-width: none;
  scroll-behavior: smooth;
}
::-webkit-scrollbar {
 width: 5px;
 height: 5px;
}
::-webkit-scrollbar-thumb {
 background: #858585;
 border-radius: 15px;
}
::-webkit-scrollbar-thumb:hover{
 background: #666666;
}
::-webkit-scrollbar-track{
 background: #ffffff;
 border-radius: 0px;
}
</style>
