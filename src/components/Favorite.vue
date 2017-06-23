<template>
  <div class="favorites">
    <div v-for="item in favorites" class="fcard">
      <div class="fcard-header">
        <div class="fcard-image">
          <img :src="item.artworkUrl30" :alt="item.artistName">
        </div>
        <div class="fcard-name">
          <p>{{ item.artistName }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'favorites',
    props: ['favorites'],
    data () {
      return {}
    },
    computed: {},
    methods: {}
  }
</script>

<style scoped>
  .favorites {
    padding: 10px 10px;
    display: flex;
    flex-direction: column;
    align-content: center;
    flex-grow: 1;
  }
  .fcard {
    display: flex;
    flex-direction: column;
    margin: 5px 0;
    background-color: #ffd4e2;
  }
  .fcard-header {
    display: flex;
    flex-direction: row;
    position: relative;
  }
  .fcard-image {
    margin: 5px;
  }
  .fcard-name {
    display: flex;
    margin-left: 20px;
    font-weight: bold;
    font-size: 20px;
  }

</style>
