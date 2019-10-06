<template>
  <div class="container">
    <h1>Pokémon's</h1>
    <PokemonSearch
      :apiUrl="api_url"
      @set-pokemon-url="setPokemonUrl($event)"
    />
    <PokemonList
      :imageUrl="image_url"
      :apiUrl="api_url"
      @set-pokemon-url="setPokemonUrl($event)"
    />
    <PokemonDetail
      v-if="show_detail"
      :imageUrl="image_url"
      :pokemonUrl="pokemon_url"
      @close-detail="closeDetail($event)"
    />
  </div>
</template>

<script>
import PokemonDetail from '@/components/PokemonDetail'
import PokemonList from '@/components/PokemonList'
import PokemonSearch from '@/components/PokemonSearch'

export default {
  components: {
    PokemonDetail,
    PokemonList,
    PokemonSearch
  },
  data() {
    return {
      api_url: 'https://pokeapi.co/api/v2/pokemon',
      image_url: 'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/',
      pokemon_url: null,
      show_detail: false
    }
  },
  methods: {
    setPokemonUrl(url) {
      this.pokemon_url = url
      this.show_detail = true

      this.keyboardListener('add')
    },
    closeDetail() {
      this.pokemon_url = null
      this.show_detail = false

      this.keyboardListener('remove')
    },
    keyboardEvent(event) {
      if (event.keyCode == 27) {
        this.closeDetail()
      }
    },
    keyboardListener(method, type ='keyup') {
      window[`${method}EventListener`](type, this.keyboardEvent)
    }
  }
}
</script>

<style lang="scss" scoped>
  @import url('https://fonts.googleapis.com/css?family=Acme');

  h1 {
    font-size: 2rem;
    font-weight: bold;
    color: #fff;
  }

  .container {
    display: flex;
    width: calc(100% - 20px);
    min-height: 100vh;
    font-family: "Acme", sans-serif;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    padding: 10px;
  }
</style>