<template>

    
  <div>
<div>
  <img style="visibility:visible;height:50%;left:18%;position:relative;top:0;width:65%"  src="../assets/images/pokemonLogo.png" />
</div>
      

    <input v-model="search" placeholder="Search by Pokémon name" />
    <div class="pokemon-container">
    <div v-for="pokemon in filteredPokemon" :key="pokemon.name">
      <nuxt-link :to="'/pokemon/' + pokemon.name">
        <img :src="pokemon.thumbnail" :alt="pokemon.name" />
        <p class="item">{{ pokemon.name }}</p>
      </nuxt-link>
    </div>
  </div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  data() {
    return {
      search: '',
      pokemonList: [], 
    };
  },
  computed: {
    filteredPokemon() {
      return this.pokemonList.filter(pokemon =>
        pokemon.name.toLowerCase().includes(this.search.toLowerCase())
      ).slice(0, 60);
    },
  },
  async mounted() {
  try {
    const limit = 60; 
    let offset = 0;
    let allPokemonFetched = false;

    while (!allPokemonFetched) {
      const response = await axios.get(`https://pokeapi.co/api/v2/pokemon?limit=${limit}&offset=${offset}`);
      const results = response.data.results;

      if (results.length === 0) {
        allPokemonFetched = true;
        break;
      }

      for (const result of results) {
        const pokemonResponse = await axios.get(result.url);
        const pokemonData = {
          name: result.name,
          thumbnail: pokemonResponse.data.sprites.front_default,
        };
        this.pokemonList.push(pokemonData);
      }

      offset += limit;
    }
  } catch (error) {
    console.error(error);
  }
},

};
</script>
<style>
@import url("https://fonts.googleapis.com/css2?family=Montserrat&display=swap");
@import url("https://fonts.cdnfonts.com/css/pokemon-solid");
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
  font-family: "Pokemon Solid", sans-serif;
}
body {
  padding: 20px;
  min-height: 100vh;
  background-color: #1b356e;
}
.pokemon-container {
  display: flex; 
  flex-wrap: wrap; 
  justify-content: space-between; 
}

.pokemon-container > div {
  width: calc(33.33% - 10px); 
  margin-bottom: 20px; 
  text-align: center; 
}
input {
  display: block;
  width: 350px;
  margin: 20px auto;
  padding: 10px 45px;
  background-size: 15px 15px;
  font-size: 16px;
  border: none;
  border-radius: 5px;
  box-shadow: rgba(50, 50, 93, 0.25) 0px 2px 5px -1px,
    rgba(0, 0, 0, 0.3) 0px 1px 3px -1px;
}
.item {
  width: 60%;
  margin: 0 auto 10px auto;
  padding: 10px 20px;
  color: #ffcc01;
  border-radius: 5px;
  box-shadow: rgba(0, 0, 0, 0.1) 0px 1px 3px 0px,
    rgba(0, 0, 0, 0.06) 0px 1px 2px 0px;
}
.pokemonLogo {
  
  position: absolute;
  padding: 10px 20px;
  left: 200px;
  margin: 20px auto;
}

</style>