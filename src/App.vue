<template>
  <div id="app">
    <h1>Vue News</h1>
    <section class="callout secondary">
      <h5 class="text-center">Filter by Category</h5>
      <form>
        <div class="row">
          <div class="large-6 columns">
            <select v-model="section">
              <option v-for="section in sections" :value="section">{{ section }}</option>
            </select>
          </div>
          <div class="medium-6 columns">
            <a @click="getPosts(section)" class="button expanded">Retrieve</a>
          </div>
        </div>
      </form>
    </section>
    <!--<img src="./assets/logo.png">-->
    <NewsList :results="results" />
  </div>
</template>

<script>
import axios from 'axios'
import NewsList from './NewsList'

const NYTBaseUrl = "https://api.nytimes.com/svc/topstories/v2/";
const ApiKey = "rO1Elou8Lw3AtxdM1wfRMGbiTC6KaAAO";

const SECTIONS = "home, arts, automobiles, books, business, fashion, food, health, insider, magazine, movies, national, nyregion, obituaries, opinion, politics, realestate, science, sports, sundayreview, technology, theater, tmagazine, travel, upshot, world"; // From NYTimes

function buildUrl (url) {
    return NYTBaseUrl + url + ".json?api-key=" + ApiKey
}

export default {
  name: 'app',
  components: { NewsList },
  data () {
    return {
        results: [],
        sections: SECTIONS.split(', '), // create an array of the sections
        section: 'home' // set default section to 'home'
    }
  },
  mounted() {
      this.getPosts('home');
  },
  methods: {
      getPosts(section) {
          let url = buildUrl(section);
          axios.get(url).then((response) => {
              this.results = response.data.results;
          }).catch( error => { console.log(error); });
      }
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
