<template>
  <aside class="poke-details">
    Details:
    <div class="poke-data-node" v-for="pokeDataKey in pokeDetailList" :key="pokeDataKey">
      <span
        style="padding-right: 8px;"
      >
        {{ pokeDataKey }}:
      </span>
      <span>
        {{ pokeDetails[pokeDataKey] }}
      </span>
    </div>
    <div>
      <div class="poke-details__ability-list">
        Pokemon Abilities:
        <div
          class="poke-ability"
          v-for="ability in pokemonAbilities"
          :key="ability.name"
          @click="showAbilityDetails(ability.name)"
        >
          {{ ability.name }}
        </div>
      </div>
      <div>
        Ability Details:
        <div v-if="!fetchingAbility">
          {{ chosenAbilityDescription }}
        </div>
        <div v-else>
          loading...
        </div>
      </div>
    </div>
  </aside>
</template>
<script>
import { BASE_POKE_API } from './PokeFetcher.vue'

export default {
  name: 'PokeDetails',
  props: {
    pokemon: {
      type: Object,
      required: false
    },
  },
  data() {
    return {
      chosenAbility: null,
      fetchingAbility: false,
    }
  },
  computed: {
    pokeDetails() {
      if (!this.pokemon) return {}
      return {
        name: this.pokemon.name,
        abilities: this.pokemon.abilities.forEach(ab => ab.ability.name).join(', '),
        weight: this.pokemon.weight,
        height: this.pokemon.height,
      }
    },
    pokeDetailList() {
      return Object.keys(this.pokeDetails)
    },
    pokemonAbilities() {
      if (!this.pokemon) return []
      return this.pokemon.abilities.map(ab => ({ ...ab.ability }))
    },
    chosenAbilityDescription() {
      if (!this.chosenAbility) return ''
      return this.chosenAbility.find(entry => entry.language.name === 'en').effect
    }
  },
  methods: {
    async showAbilityDetails(name) {
      if (this.fetchingAbility) return
      this.fetchingAbility = true
      try {
        const pokeAbility = await fetch(`${BASE_POKE_API}ability/${name}`)
        const abilityDetails = await pokeAbility.json()
        this.chosenAbility = abilityDetails
      } catch (err) {
        console.error(err)
      } finally {
        this.fetchingAbility = false
      }
    }
  },
}
</script>
<style scoped>
.poke-details {
  font-size: 20px;
  width: 20%;
  background: #00afb9;
  padding: 40px;
}
.poke-data-node {
  display: flex;
  justify-content: space-between;
  margin: 0 8px;
}
.poke-ability {
  cursor: pointer;
}
.poke-ability:hover {
  color: white;
}
.poke-details__ability-list {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  padding: 8px;
  background: #f9c74f;
  border: 1px solid #0096c7;
  border-radius: 3px;
  margin: 10px 0;
}
</style>
