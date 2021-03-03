<template>
  <div id="app" onload="mostrar()">

    <div class=" container is-desktop" id="conteudo"> 
      
      <div id="imagem">
        <img src="./assets/pokedex.png" alt="">
      </div>

      <hr>
      <input type="text" placeholder="Busque seu pokemon!" v-model="busca" class="input is-rounded">
      <button class="button is-fullwidth is-success" id="buscaBtn" >Buscar</button> 

      <div class="columns is-multiline ">
        <div v-for="(poke, index) in buscarPoke" :key="poke.url" class="column is-one-third">
          <Pokemon :name="poke.name" :url="poke.url" :num="index+1"/>
        </div>
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
    
    buscarPoke: function(){
      if(this.busca == '' || this.busca == " "){
        return this.pokemons;
      }else{
        return this.pokemons.filter(pokemon => pokemon.name.includes(this.busca))
      }

    }
  },
  methods:{
    /*buscar: function(){
      this.filteredPokemons = this.pokemons;
      
      if(this.busca == '' || this.busca == " "){
        this.filteredPokemons = this.pokemons;
      }else{
        this.busca = this.busca.toLowerCase();
        this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name == this.busca)
      }
      
    },*/
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
