<template>
  <div id="app">
    <h1>Vue News</h1>
    <!--<img src="./assets/logo.png">-->
    <FilterForm :getPosts="getPosts" />
    <NewsList :results="results" />
  </div>
</template>

<script>
import axios from 'axios'
import NewsList from './components/NewsList'
import FilterForm from './components/FilterForm'

const NYTBaseUrl = "https://api.nytimes.com/svc/topstories/v2/";
const ApiKey = "rO1Elou8Lw3AtxdM1wfRMGbiTC6KaAAO";

function buildUrl (url) {
    return NYTBaseUrl + url + ".json?api-key=" + ApiKey
}

export default {
  name: 'app',
  components: {
      NewsList,
      FilterForm
  },
  data () {
    return {
        results: []
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
