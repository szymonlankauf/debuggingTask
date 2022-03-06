<template>
<div class="poke-container">
  <main class="poke-fetcher">
    <button
      class="poke-btn"
      :class="{ 'poke-btn--disabled': fetchingPokemon }"
      :disabled="fetchingPokemon"
      @click="getRandomPokemon"
    >
      Get pokemon
    </button>
    <div class="poke-list">
      Pokemons:
      <ul>
        <li class="poke-list__node" v-for="pokemon in pokemonList" :key="pokemon.id">
          <div class="pokemon-name">
            {{ pokemon.name }}
            <button class="poke-btn poke-list__node-btn" @click="showDetails(pokemon.id)">Show details</button>
          </div>
        </li>
      </ul>
    </div>

  </main>
  <PokeDetails :pokemon="chosenPokemon" />
</div>
</template>

<script>
import PokeDetails from './PokeDetails.vue'

export const BASE_POKE_API = 'https://pokeapi.co/api/v2/'

export default {
  name: 'PokeFetcher',
  components: {
    PokeDetails,
  },
  data() {
    return {
      fetchingPokemon: false,
      pokemonData: {},
      chosenPokemonId: null
    }
  },
  computed: {
    pokemonList() {
      return Object.values(this.pokemonData)
    },
    chosenPokemon() {
      return this.pokemonData[this.chosenPokemonId]
    }
  },
  methods: {
    async getPokemon(pokeApiData) {
      if (this.fetchingPokemon) return
      this.fetchingPokemon = true
      try {
        const pokeData = await fetch(`${BASE_POKE_API}/${pokeApiData}`)
        const pokemon = pokeData.json()
        this.pokemonData[pokemon.id] = pokemon
      } catch (err) {
        console.error(err)
      }
    },
    getRandomPokemon() {
      const id = Math.floor(100 * Math.random() + 1)
      this.getPokemon(id)
    },
    showDetails(pokemonId) {
      this.chosenPokemonId = pokemonId
    }
  }
}
</script>

<style scoped>
.poke-container {
  min-height: 1200px;
  display: flex;
}
.poke-fetcher {
  width: 80%;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
  padding: 40px;
}
.poke-list {
  padding: 40px;
}
.poke-list__node {
  height: 24px;
  padding: 6px 0;
  border-bottom: 1px solid #ced4da;
}
.pokemon-name {
  display: flex;
  width: 400px;
  justify-content: space-between;
}
.poke-list__node-btn {
  display: none;
}
.poke-list__node:hover .poke-list__node-btn {
  display: block;
}
</style>
