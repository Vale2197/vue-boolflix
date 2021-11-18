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
        <!-- search form -->

        <div class="searchResult">
          
          <div v-for="film in userFilms" :key="film.id" class="film">
            <p>
              TITOLO: {{film.title}}
            </p>
            <!--  -->

            <p>
              TITOLO ORIGINALE: {{film.original_title}}
            </p>
            <!--  -->

            <p>
              LINGUA: {{film.original_language}}
            </p>
            <!--  -->

            <p>
              VOTO: {{film.vote_average}}
            </p>
            <!--  -->
            
            <p>
              --------------------------------------- <br>
            </p>
          </div>

        </div>
        <!--  -->
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
        userFilms: [],
      }
    },
    methods: {
      startSearch(){
        axios.get(`https://api.themoviedb.org/3/search/movie?api_key=d96fb085a5d3052af443a4f202383b1f&query=${this.userQuery}&page=1`)
            .then(r => {
              console.log(r.data.results);
              this.userFilms = r.data.results;
            })
      }
    },
}
</script>

<style lang="scss">

</style>
