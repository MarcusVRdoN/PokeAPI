<template>
  <div class="list">
    <article v-for="(pokemon, index) in pokemons" :key="index" @click="setPokemonUrl(pokemon.url)">
      <img :src="`${imageUrl}${pokemon.id}.png`" width="96" height="96">
      <h3>{{ pokemon.name }}</h3>
    </article>
    <div ref="infiniteScrollTrigger" id="scroll-trigger">
      <i class="fas fa-spinner fa-spin"></i>
    </div>
  </div>
</template>

<script>
export default {
  props: [
    'apiUrl',
    'imageUrl'
  ],
  data() {
    return {
      pokemons: [],
      next_url: null,
      current_url: null
    }
  },
  created() {
    this.current_url = this.apiUrl
    this.fetchData()
  },
  methods: {
    fetchData() {
      let req = new Request(this.current_url)

      fetch(req)
        .then(resp => {
          if (resp.status == 200) 
            return resp.json()
        })
        .then(data => {
          this.next_url = data.next
          data.results.forEach(pokemon => {
            pokemon.id = pokemon.url.split('/').filter(part => !!part).pop()
            this.pokemons.push(pokemon)
          })
        })
        .catch(error => console.log(error))
    },
    scrollTrigger() {
      const observer = new IntersectionObserver(entries => {
        entries.forEach(entry => {
          if (entry.intersectionRatio > 0 && this.next_url) {
            this.next()
          }
        })
      })

      observer.observe(this.$refs.infiniteScrollTrigger)
    },
    next() {
      this.current_url = this.next_url
      this.fetchData()
    },
    setPokemonUrl(url) {
      this.$emit('set-pokemon-url', url)
    }
  },
  mounted() {
    this.scrollTrigger()
  }
}
</script>

<style lang="scss" scoped>
  .list {
    display: grid;
    width: 100%;
    max-width: 510px;
    grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
    grid-gap: 10px;

    article {
      height: 150px;
      background-color: #efefef;
      text-align: center;
      text-transform: capitalize;
      border-radius: 5px;
      cursor: pointer;
      box-shadow: 0 15px 30px rgba(0, 0, 0, .2), 0 10px 10px rgba(0, 0, 0, .2);

      h3 {
        margin: 0;
      }
    }
  }

  #scroll-trigger {
    display: flex;
    width: 100%;
    height: 150px;
    justify-content: center;
    align-items: center;
    font-size: 2rem;
    color: #efefef;
  }
</style>