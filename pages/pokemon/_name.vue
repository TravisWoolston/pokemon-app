<template>
  <div style="font-size: 30px">
    <img :src="pokemon.sprites.front_shiny" :alt="pokemon.name" />
    <p>Name: {{ pokemon.name }}</p>
    <p>Height: {{ pokemon.height }}</p>
    <p>Weight: {{ pokemon.weight }}</p>
    <p>Abilities: {{ formattedAbilities }}</p>
    <nuxt-link style="color=red;" to="/"><img style="max-width: 50px" src="https://upload.wikimedia.org/wikipedia/commons/thumb/5/53/Pok%C3%A9_Ball_icon.svg/2052px-Pok%C3%A9_Ball_icon.svg.png">Back to Pokémon List</nuxt-link>
  </div>
</template>

<script>
import axios from "axios";

export default {
  async asyncData({ params }) {
    try {
      const response = await axios.get(
        `https://pokeapi.co/api/v2/pokemon/${params.name}`
      );
      const pokemonData = response.data;
      console.log(pokemonData);
      return { pokemon: pokemonData };
    } catch (error) {
      console.error(error);
      return { pokemon: null };
    }
  },
  computed: {
    formattedAbilities() {
      return this.pokemon.abilities
        .map(
          (ability) =>
            ability.ability.name.charAt(0).toUpperCase() +
            ability.ability.name.slice(1)
        )
        .join(", ");
    },
  },
};
</script>
