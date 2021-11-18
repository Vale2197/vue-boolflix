<template>
  <div id="app">

     <header>
      <div class="container">
        <div class="row">
          <div class="col-12">
            <input v-model="userQuery" type="text" placeholder="search" id="search">
            <button @click="startSearch">
              search
            </button>
          </div>
        </div>
      </div>
  </header>
    <!--  -->

  </div>
</template>

<script>
import axios from "../node_modules/axios"
export default {
  name: 'App',

    data() {
      return {
        userQuery: "",
      }
    },
    methods: {
      startSearch(){
        axios.get(`https://api.themoviedb.org/3/search/movie?api_key=d96fb085a5d3052af443a4f202383b1f&query=${this.userQuery}&page=1`)
            .then(r => {
              console.log(r);
            })
      }
    },
}
</script>

<style lang="scss">

</style>
