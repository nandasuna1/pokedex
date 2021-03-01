<template>
  <div id="app" onload="mostrar()">

    <div class="column is-half is-offset-one-quarter" id="conteudo"> 
      
      <div id="imagem">
        <img src="../public/poke-logo.jpg" alt="">
      </div>

      <hr>
      <h4 id="is-size-4">Pokedex</h4>
      <input type="text" placeholder="Busque seu pokemon!" v-model="busca" class="input is-rounded">
      <button class="button is-fullwidth is-success" id="buscaBtn" @click="buscar">Buscar</button>   
      <div v-for="(poke, index) in filteredPokemons" :key="poke.url">
        <Pokemon :name="poke.name" :url="poke.url" :num="index+1"/>
      </div>

    </div>

  </div>
</template>

<script>
import axios from 'axios';
import Pokemon from './components/Pokemon';
export default {
  name: 'App',

  data(){
    return{
      pokemons:[],
      busca: '',
      filteredPokemons: [],
    }
  },
  //tudo posto no created carrega junto com a pasta
  created: function(){
    axios.get('https://pokeapi.co/api/v2/pokemon?limit=151&offset=0').then(res => {
      console.log("pegou");
      this.pokemons = res.data.results;
      console.log(this.pokemons);
      this.filteredPokemons = res.data.results;
      this.mostrar
    })
  },
  components:{
    Pokemon 
  },

  computed:{
    /*
    resultadoBusca: function(){
      if(this.busca == '' || this.busca == " "){
        return this.pokemons;
      }else{
        return this.pokemons.filter(pokemon => pokemon.name == this.busca)
      }

    }*/
  },
  methods:{
    buscar: function(){
      this.filteredPokemons = this.pokemons;
      
      if(this.busca == '' || this.busca == " "){
        this.filteredPokemons = this.pokemons;
      }else{
        this.busca = this.busca.toLowerCase();
        this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name == this.busca)
      }
      
    },
    mostrar: function(){
      return this.pokemons
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

#buscaBtn{
  margin-top: 2%;
}
</style>
