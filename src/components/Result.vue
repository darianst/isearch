<template>
  <div class="search-results">
    <div v-for="item in items" class="card">
      <div class="card-header">
        <div class="fav-icon"><img src="../assets/fav.png" @click="addToFavorite(item)"></div>
        <div class="card-kind">{{ item.kind }}</div>
        <div class="card-image">
          <img :src="item.artworkUrl60" :alt="item.artistName">
        </div>
        <div class="card-name">
          <p>{{ item.artistName }}</p>
        </div>
      </div>
      <div v-if="item.kind === 'song'" class="card-description">
        <p>{{ item.collectionName }}</p>
      </div>
      <div v-if="item.kind === 'feature-movie'" class="card-description">
        <p>{{ item.shortDescription }}</p>
      </div>
      <div v-if="item.kind === 'music-video'" class="card-description">
        <p>{{ item.trackName }}</p>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'result',
    props: ['items'],
    data () {
      return {}
    },
    computed: {},
    methods: {
      addToFavorite (item) {
        this.$emit('addToFavorite', item)
      }
    }
  }
</script>

<style scoped>
  .search-results {
    padding: 10px 20px;
    display: flex;
    flex-direction: column;
    align-content: center;
    flex-grow: 1;
  }
  .card {
    display: flex;
    flex-direction: column;
    margin: 5px 0;
    background-color: #c0e3ff;
  }
  .card-header {
    display: flex;
    flex-direction: row;
    position: relative;
  }
  .fav-icon {
    position: absolute;
    top: 10px;
    right: 10px;
    cursor: pointer;
  }
  .card-kind {
    position: absolute;
    bottom: 10px;
    right: 100px;
  }
  .card-image {
    margin: 5px;
  }
  .card-name {
    display: flex;
    margin-left: 20px;
    font-weight: bold;
    font-size: 20px;
  }
  .card-description {
    display: flex;
    padding-left: 20px;
  }

</style>
