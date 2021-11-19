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
        </div>
      </header>
      <!-- 
        HEADER
       -->
       <div class="container">

            <div class="searchResult"> 
              <h2>
                FILMS:
              </h2>
                <div v-for="film in userFilms[0]" :key="film.id" class="film">

                  <img :src="film.poster_path == null && film.backdrop_path == null ? 'https://picsum.photos/300/200' :
                  film.poster_path == null ? poster_prefix + '/w300' + film.backdrop_path 
                  : poster_prefix + '/w300' + film.poster_path " 
                  alt="poster"
                  >

                  <p>
                    TITOLO: {{film.title}}
                  </p>
                  <!--  -->

                  <p>
                    TITOLO ORIGINALE: {{film.original_title}}
                  </p>
                  <!--  -->

                  <p>
                    LINGUA: {{film.original_language}} <CountryFlag :country="film.original_language == 'en' ? 'gb' : film.original_language"/>
                  </p>
                  <!--  -->

                  <p>
                    VOTO:
                    <i v-for="filledStar in voto(film) " :key="filledStar" class="fas fa-star"></i> 
                    <i v-for="emptyStar in 5 - voto(film)" :key="emptyStar" class="far fa-star"></i>
                  </p>
                  <!--  -->
                  
                  <p>
                    --------------------------------------- <br>
                  </p>
                </div>
            </div>
            <!-- 
                  / FILMS
            -->
            <div class="searchResult"> 
              <h2>
                SERIES:
              </h2>
                <div v-for="serie in userSeries[0]" :key="serie.id" class="film">
                  <img :src="serie.poster_path == null && serie.backdrop_path == null ? 'https://picsum.photos/300/500' :
                  serie.poster_path == null ? poster_prefix + '/w300' + serie.backdrop_path 
                  : poster_prefix + '/w300' + serie.poster_path">
                  <p>
                    TITOLO: {{serie.name}}
                  </p>
                  <!--  -->

                  <p>
                    TITOLO ORIGINALE: {{serie.original_name}}
                  </p>
                  <!--  -->

                  <p>
                    LINGUA: {{serie.original_language}} <CountryFlag :country="serie.original_language == 'en' ? 'gb' : serie.original_language"/>
                  </p>
                  <!--  -->

                  <p>
                    VOTO:
                    <i v-for="filledStar in voto(serie) " :key="filledStar" class="fas fa-star"></i> 
                    <i v-for="emptyStar in 5 - voto(serie)" :key="emptyStar" class="far fa-star"></i>
                  </p>
                  <!--  -->
                  
                  <p>
                    --------------------------------------- <br>
                  </p>
                </div>
            </div>
            <!-- 
                /SERIES
            -->

       </div>
       <!-- 
            /container
        -->
  </div>
</template>

<script>
import axios from "../node_modules/axios";
import CountryFlag from '../node_modules/vue-country-flag';

export default {
  name: 'App',

    components: {
      CountryFlag
    },

    data() {
      return {
        userQuery: "",
        userFilms:  [],
        userSeries: [],
        seriesUrlFirstPart: `https://api.themoviedb.org/3/search/tv?api_key=d96fb085a5d3052af443a4f202383b1f&page=1&query=`,
        filmUrlFirstPart: `https://api.themoviedb.org/3/search/movie?api_key=d96fb085a5d3052af443a4f202383b1f&query=`,
        filmUrlLastPart: `&page=1`,
        poster_prefix: 'https://image.tmdb.org/t/p/',
      }
    },
    methods: {
      
      startSearch(){
        this.userFilms = [],
        this.userSeries = [],
        this.apiCall(this.userFilms, this.filmUrlFirstPart, this.userQuery, this.filmUrlLastPart);
        this.apiCall(this.userSeries, this.seriesUrlFirstPart, this.userQuery, "")
      },

      apiCall(array, linkFirstPart, userInput, linkLast) {
        console.log(userInput);
        axios.get(linkFirstPart + userInput + linkLast).then(r => {
          console.log(r.data.results);
            array.push(r.data.results);
            console.log(array);
        })
        .catch(err => {
          console.log(err + 'errore');
        })
      },

      voto(film) {
          let votoIntero = Math.ceil(film.vote_average)
          if(votoIntero > 5) {
             return votoIntero = 5;
          }

          return votoIntero;
        }

    },
    computed: {
        
    }
}
</script>

<style lang="scss">

</style>
