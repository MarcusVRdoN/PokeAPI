<template>
  <div id="app">
    <Pokemon />
  </div>
</template>

<script>
import Pokemon from '@/components/Pokemon'

export default {
  name: 'app',
  components: {
    Pokemon
  }
}
</script>

<style>
  @import url('https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.11.2/css/all.min.css');

  body {
    margin: 0;
  }

  #app {
    background-image: radial-gradient(circle, #3f6cb0, #2a57aa, #1d42a2, #1e2996, #260487);
    min-height: 100vh;
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    text-align: center;
    color: #2c3e50;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }
</style>
