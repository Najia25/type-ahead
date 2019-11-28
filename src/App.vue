<template>
  <div id="app">
    <SearchForm
    v-if="cities.length"
    v-bind:cities="cities"
    />
  </div>
</template>

<script>
import SearchForm from './components/SearchForm.vue'

export default {
  name: 'app',
  components: {
    SearchForm
  },
  data() {
    return {
      cities: []
    }
  },
  mounted() {
    fetch('https://gist.githubusercontent.com/Miserlou/c5cd8364bf9b2420bb29/raw/2bf258763cdddd704f8ffd3ea9a3e81d25e2c6f6/cities.json')
    .then(response => response.json())
    .then((data) => {
      this.cities.push(...data)
      // console.log(this.cities)
      // this.cities = data
      // setTimeout(() => {
      //   console.log(this.cities)
      // }, 3000);
      });
  }
}
</script>

<style>

html {
  box-sizing: border-box;
  background: #ffc600;
  font-family: 'helvetica neue';
  font-size: 20px;
  font-weight: 200;
}

*, *:before, *:after {
  box-sizing: inherit;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

</style>
