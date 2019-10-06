<template>
  <div class="detail" @click="clickOutside($event)">
    <div class="detail__view" v-if="show">
      <div class="detail__view__image" v-if="pokemon">
        <img :src="`${imageUrl}${pokemon.id}.png`" width="96" height="96">
      </div>
      <div class="detail__view__data" v-if="pokemon">
        <h2>{{ pokemon.name }}</h2>
        <div class="property">
          <div class="left">Base Experience</div>
          <div class="right">{{ pokemon.base_experience }} XP</div>
        </div>
        <div class="property">
          <div class="left">Height</div>
          <div class="right">{{ pokemon.height / 10 }}m</div>
        </div>
        <div class="property">
          <div class="left">Weight</div>
          <div class="right">{{ pokemon.weight / 10 }}Kg</div>
        </div>
        <h3>Pokemon Types</h3>
        <div class="types">
          <div class="type" v-for="(value, index) in pokemon.types" :key="index">
            {{ value.type.name }}
          </div>
        </div>
        <h3>Abilities</h3>
        <div class="abilities">
          <div class="ability" v-for="(value, index) in pokemon.abilities" :key="index">
            {{ value.ability.name }}
          </div>
        </div>
      </div>
      <h2 v-else>The pokemon was not found</h2>
      <button class="close" @click="closeDetail()">close</button>
    </div>
    <i v-else class="fas fa-spinner fa-spin"></i>
  </div>
</template>

<script>
export default {
  props: [
    'pokemonUrl',
    'imageUrl'
  ],
  data() {
    return {
      show: false,
      pokemon: {}
    }
  },
  methods: {
    fetchData() {
      let req = new Request(this.pokemonUrl)

      fetch(req)
        .then(resp => {
          if (resp.status == 200) 
            return resp.json()
        })
        .then(data => {
          this.pokemon = data
          this.show = true
        })
        .catch(error => console.log(error))
    },
    closeDetail() {
      this.$emit('close-detail')
    },
    clickOutside(event) {
      if (event.target === event.currentTarget) {
        this.$emit('close-detail')
      }
    }
  },
  created() {
    this.fetchData()
  }
}
</script>

<style lang="scss" scoped>
  .detail {
    position: fixed;
    background-color: rgba($color: #000, $alpha: .7);
    display: flex;
    width: calc(100% - 20px);
    height: calc(100vh - 20px);
    justify-content: flex-start;
    align-items: center;
    flex-direction: column;
    top: 0;
    bottom: 0;
    padding: 90px 10px 10px;

    &__view {
      position: relative;
      background-color: #fff;
      display: flex;
      width: 100%;
      max-width: 510px;
      justify-content: center;
      align-items: center;
      flex-direction: column;
      padding: 60px 10px 10px;
      border-radius: 5px;
      box-shadow: 0 15px 30px rgba(0, 0, 0, .2), 0 10px 10px rgba(0, 0, 0, .2);

      &__image {
        position: absolute;
        background-color: #333;
        display: flex;
        width: 120px;
        height: 120px;
        justify-content: center;
        align-items: center;
        top: -60px;
        border-radius: 50%;
        box-shadow: 0 15px 30px rgba(0, 0, 0, .2), 0 10px 10px rgba(0, 0, 0, .2);
        overflow: hidden;
      }

      h2 {
        text-transform: capitalize;
      }

      &__data {
        display: flex;
        width: 100%;
        justify-items: flex-start;
        align-items: center;
        flex-direction: column;
        margin-bottom: 40px;

        .property {
          width: 90%;
          max-width: 400px;
          border-bottom: 1px solid #ccc;
          margin-bottom: 10px;
        }

        .left {
          float: left;
        }

        .right {
          float: right;
        }
      }

      h3 {
        width: 90%;
        max-width: 400px;
        border-bottom: 1px solid #ccc;
      }

      .types,
      .abilities {
        display: flex;
        width: 90%;
        max-width: 400px;
        justify-content: center;
        flex-wrap: wrap;

        .type,
        .ability {
          font-size: 1rem;
          text-transform: capitalize;
          color: #fff;
          padding: 5px 10px;
          margin: 0 10px 10px 0;
          border-radius: 20px;
          letter-spacing: 2px;
          word-wrap: none;
          word-break: keep-all;
        }

        .type {
          background-color: #0a2e50;
        }

        .ability {
          background-color: #c73015;
        }
      }
      .close {
        background-color: #333;
        font-size: 0.8rem;
        text-transform: capitalize;
        color: #efefef;
        padding: 10px 20px;
        margin-bottom: 15px;
        border: none;
        border-radius: 5px;
        cursor: pointer;
        outline: none;
      }
    }

    i {
      font-size: 2rem;
      color: #efefef;
    }
  }

  .slide-y-reverse-transition-enter-active,
  .slide-y-reverse-transition-leave-active {
    transition: 0.3s cubic-bezier(0.25, 0.8, 0.5, 1);
  }
  .slide-y-reverse-transition-move {
    transition: transform 0.6s;
  }
  .slide-y-reverse-transition-enter,
  .slide-y-reverse-transition-leave-to {
    opacity: 0;
    transform: translateY(15px);
  }
</style>