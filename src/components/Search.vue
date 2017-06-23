<template>
  <div>
    <div>
        <div class="search-wrapper">
          <input required class="" type="text" id="search-term" autocomplete="off" v-model="search">
          <button type="button" id="search-button" class="btn" @click="getResults()">
            Search
          </button>
        </div>
    </div>
    <div id="content" v-infinite-scroll="addNextTen" infinite-scroll-distance="10">
      <div v-if="results" id="results">
        <result :items="items" @addToFavorite="addToFavorite"></result>
        <div id="bottom-edge"
        ></div>
      </div>
      <div v-if="!results" class="no-result">
        <span>No result</span>
      </div>
      <div id="favorite">
        <favorites :favorites="favorites"></favorites>
      </div>
    </div>
  </div>
</template>

<script>
  const axios = require('axios')
  import Result from './Result'
  import Favorites from './Favorite'
  import infiniteScroll from 'vue-infinite-scroll'

  const chunk = (arr, chunkSize) => {
    var R = []
    for (var i = 0, len = arr.length; i < len; i += chunkSize) {
      R.push(arr.slice(i, i + chunkSize))
    }
    return R
  }

  export default {
    name: 'search',
    components: {
      Favorites,
      Result
    },
    directives: { infiniteScroll },
    data () {
      return {
        search: '',
        items: [],
        favorites: [],
        searchResults: []
      }
    },
    computed: {
      results: function () {
        return !!(this.items || []).length
      }
    },
    methods: {
      getResults: function () {
        if (this.search.length < 3) return
        axios.get('https://itunes.apple.com/search?term=' + this.search.split(' ').join('+'))
          .then((response) => {
            this.searchResults = chunk(response.data.results, 10)
            if (this.searchResults.length) {
              this.items = this.searchResults[0]
            }
          })
          .catch((error) => {
            throw new Error(`Error! Cannot connect to API. ${error}`)
          })
      },

      addToFavorite: function (item) {
        this.favorites.push(item)
      },

      addNextTen: function () {
        if (this.items.length) {
          if (this.searchResults.length) {
            this.searchResults.shift()
          }
          if (this.searchResults.length && this.searchResults[0].length) {
            this.searchResults[0].map((item) => {
              this.items.push(item)
            })
          }
        }
      }
    },
    created () {
      this.addNextTen()
    }
  }
</script>

<style scoped>
  .search-wrapper {
    display: flex;
    flex-direction: row;
    justify-content: center;
  }
  #search-term {
    display: flex;
    line-height: 20px;
    font-size: 20px;
    width: 90%;
    text-indent: 10px;
  }
  #search-button {
    font-weight: bold;
    display: flex;
    height: 30px;
    margin-left: 5px;
  }
  #content {
    margin-top: 20px;
    display: flex;
    flex-direction: row;
  }
  #results {
    display: flex;
    width: 60%;
  }
  #favorite {
    display: flex;
    width: 35%;
  }
  .no-result {
    display: flex;
    flex-grow: 2;
    text-align: center;
  }
  .no-result span {
    margin: auto;
  }
</style>
