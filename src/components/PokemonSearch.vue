<template>
  <div class="searchbar">
    <form @submit.prevent="setPokemonUrl()">
      <input type="text" v-model="search_value">
    </form>
    <i class="fas fa-search" @click="setPokemonUrl()"></i>
  </div>
</template>

<script>
export default {
  props: [
    'apiUrl'
  ],
  data() {
    return {
      search_value: null
    }
  },
  methods: {
    setPokemonUrl() {
      if (this.search_value) {
        this.$emit('set-pokemon-url', `${this.apiUrl}/${this.search_value.toLowerCase()}`)
      }
    }
  }
}
</script>

<style lang="scss" scoped>
  .searchbar {
    position: relative;
    width: 100%;
    max-width: 510px;
    padding-bottom: 20px;

    input {
      width: calc(100% - 50px);
      font-size: 1rem;
      padding: 10px 40px 10px 10px;
      border: none;
      border-radius: 5px;
      outline: none;
      box-shadow: 0 15px 30px rgba(0, 0, 0, .2), 0 10px 10px rgba(0, 0, 0, .2);
    }

    i {
      position: absolute;
      font-size: 1.25rem;
      color: #0a2e50;
      top: 10px;
      right: 10px;
      cursor: pointer;
    }
  }
</style>