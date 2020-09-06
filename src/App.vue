<template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter">
      <img src="./assets/pokemon-logo.png" style="max-width: 35%">
      <h4 class="is-size-4">Pokedex</h4>
      <hr>
      <div :class="{'control is-medium is-loading':showLoad}">
        <!-- analisar a implementação de search loading ao clicar em busca -->
        <input type="text" class="input is-rounded" placeholder="Buscar pokemon pelo nome" v-model="busca">
        <button class="button is-rounded" id="buscarBtn" @click="buscar">Buscar</button>
      </div>
      <div v-for="(poke,index) in filteredPokemons" :key="poke.url">
        <Pokemon :name="poke.name" :url="poke.url" :num="index + 1"/>
      </div>
    </div>
  </div>
</template>

<script>

import axios from 'axios'
import Pokemon from './components/Pokemon.vue'
export default {
  name: 'App',
  data(){
    return {
      pokemons: [],
      filteredPokemons: [],
      showLoad: false,
      busca: ''
    }
  },
  created: function(){
    axios.get('https://pokeapi.co/api/v2/pokemon?limit=151&offset=0').then(res => {
      console.log("Pegou a lista de Pokemons");
      this.pokemons = res.data.results;
      this.filteredPokemons = res.data.results;

    })
  },
  methods: {
    
    buscar: function(){

      this.filteredPokemons = this.pokemons;
      if(this.busca == '' || this.busca == ' ' || this.busca == null){
        this.filteredPokemons = this.pokemons;
        
      }else{
        this.showLoad = true
        
        this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name.toUpperCase() == this.busca.toUpperCase())
          
      }      
    }

  },
  components: {
    Pokemon    
  },
  computed: {
    // resultadoBusca: function(){

    //   if(this.busca == '' || this.busca == ' ' || this.busca == null){
          
    //       return this.pokemons;

    //   }else{

    //     return this.pokemons.filter(pokemon => pokemon.name == this.busca )

    //   }

    // }
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
#buscarBtn{
  margin-top: 2%;
}
</style>
