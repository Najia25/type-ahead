<template>
  <form class="search-form">
    <input type="text" class="search" placeholder="City or State" v-model="searchItem" @change="displayMatches" @keyup="displayMatches">
    <ul class="suggestions">
      <li>Filter for a city</li>
      <li>or a state</li>
    </ul>
  </form>
</template>

<script>
export default {
  name: 'SearchForm',
  data () {
    return {
      searchItem:''
    }
  },
  props: {
    cities: Array
  },
  methods: {

    findMatches(wordToMatch, cities) {
      return cities.filter(place => {
    // here we need to figure out if the city or state matches what was searched
      const regex = new RegExp(wordToMatch, 'gi');
      return place.city.match(regex) || place.state.match(regex)
  })
    },

    numberWithCommas(x) {
       return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',');
    },

    displayMatches(){
      const matchArray = this.findMatches(this.searchItem, this.cities);
      const html = matchArray.map(place => {
      const regex = new RegExp(this.searchItem, 'gi');
      const cityName = place.city.replace(regex, `<span class="hl">${this.searchItem}</span>`);
      const stateName = place.state.replace(regex, `<span class="hl">${this.searchItem}</span>`);
      return `
        <li>
          <span class="name">${cityName}, ${stateName}</span>
          <span class="population">${this.numberWithCommas(place.population)}</span>
        </li>
      `;
      }).join('');
      const suggestions = document.querySelector('.suggestions');
      console.log(suggestions)
      suggestions.innerHTML = html;
    }
    
  }

}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>

input {
  width: 100%;
  padding: 20px;
}

.search-form {
  max-width: 400px;
  margin: 50px auto;
}

input.search {
  margin: 0;
  text-align: center;
  outline: 0;
  border: 10px solid #F7F7F7;
  width: 120%;
  left: -10%;
  position: relative;
  top: 10px;
  z-index: 2;
  border-radius: 5px;
  font-size: 40px;
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.12), inset 0 0 2px rgba(0, 0, 0, 0.19);
}

.suggestions {
  margin: 0;
  padding: 0;
  position: relative;
  /*perspective: 20px;*/
}

.suggestions li {
  background: white;
  list-style: none;
  border-bottom: 1px solid #D8D8D8;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.14);
  margin: 0;
  padding: 20px;
  transition: background 0.2s;
  display: flex;
  justify-content: space-between;
  text-transform: capitalize;
}

.suggestions li:nth-child(even) {
  transform: perspective(100px) rotateX(3deg) translateY(2px) scale(1.001);
  background: linear-gradient(to bottom,  #ffffff 0%,#EFEFEF 100%);
}

.suggestions li:nth-child(odd) {
  transform: perspective(100px) rotateX(-3deg) translateY(3px);
  background: linear-gradient(to top,  #ffffff 0%,#EFEFEF 100%);
}

span.population {
  font-size: 15px;
}

.hl {
  background: #ffc600;
}

</style>
