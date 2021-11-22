<template>
  <div id="app">
       <header>
        <div class="container">

            <div class="row py-3 align-items-center">
              <div class="logo col-3 text-start">
                BOOLFLIX
              </div>
              <div class="col-9 text-end">
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
       <div class="container-fluid">

            <div v-if="userFilms.length > 0" class="searchResult container"> 

              <div class="row justify-center">
                  <h2 class="col-12 text-center">
                    FILMS:
                  </h2>
                <!-- film card -->
                <div v-for="film in userFilms[0]" :key="film.id" class="film col-4 ">

                  <div class="card" :style="background(film)">
                  </div>
                      <!-- 
                        FILM DESCRIPTION
                       -->
                          <div class="description">
                              <p>
                               <strong> TITOLO: </strong> {{film.title}}
                              </p>
                              <!--  -->

                              <p>
                                 <strong> TITOLO ORIGINALE: </strong> {{film.original_title}}
                              </p>
                              <!--  -->

                              <p v-if="flags.includes(film.original_language)" class="lingua">

                                 <strong> LINGUA: </strong><CountryFlag :country="film.original_language == 'en' ? 'gb' 
                                : film.original_language == 'ja' ? 'jp' : film.original_language"
                                />
                              </p>
                              <!-- / lingua bandiera --> 
                              <p v-else>
                                 <strong> LINGUA: </strong> {{film.original_language}}
                              </p>
                              <!-- / lingua paragrafo -->
                              <!--  -->
                              <p v-if="film.overview.length > 0 && film.overview !== ''">
                                 <strong> OVERVIEW: </strong> {{film.overview}}
                              </p>

                              <p>
                                 <strong> VOTO: </strong>
                                <i v-for="filledStar in voto(film) " :key="filledStar" class="fas fa-star"></i> 
                                <i v-for="emptyStar in 5 - voto(film)" :key="emptyStar" class="far fa-star"></i>
                              </p>
                              <!--  -->
                          </div>
                       <!-- 
                         // FILM DESCRIPTION
                        -->
                  
                </div>
                <!-- // film card -->
              </div>
              
            </div>
            <!-- 
                  / FILMS
            -->
            <div v-if="userSeries.length > 0" class="searchResult container"> 

              <div class="row justify-center">
                  <h2 class="col-12 text-center">
                    SERIES:
                  </h2>
                <!-- serie card -->
                <div v-for="serie in userSeries[0]" :key="serie.id" class="film col-4 ">

                  <div class="card" :style="background(serie)">
                  </div>
                  <!-- 
                    description
                   -->
                  <div class="description">
                      <p>
                      <strong> TITOLO: </strong> {{serie.name}}
                      </p>
                      <!--  -->

                      <p>
                        <strong> TITOLO ORIGINALE: </strong> {{serie.original_name}}
                      </p>
                      <!--  -->

                      <p v-if="flags.includes(serie.original_language)" class="lingua">

                        <strong> LINGUA: </strong><CountryFlag :country="serie.original_language == 'en' ? 'gb' 
                        : serie.original_language == 'ja' ? 'jp' : serie.original_language"
                        />
                      </p>
                      <!-- / lingua bandiera --> 
                      <p v-else>
                        <strong> LINGUA: </strong> {{serie.original_language}}
                      </p>
                      <!-- / lingua paragrafo -->
                      <p v-if="serie.overview.length > 0 && serie.overview !== ''">
                          <strong>  OVERVIEW: </strong> {{serie.overview}}
                      </p>
                      
                      <p>
                        <strong> VOTO: </strong>
                        <i v-for="filledStar in voto(serie) " :key="filledStar" class="fas fa-star"></i> 
                        <i v-for="emptyStar in 5 - voto(serie)" :key="emptyStar" class="far fa-star"></i>
                      </p>
                      <!--  -->
                  </div>
                  <!-- 
                    // description
                   -->
                  
                </div>
                <!-- // serie card -->
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
        flags: ['it', 'en', 'ja', 'de', 'fr', 'es',],
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
        //console.log(userInput);
        axios.get(linkFirstPart + userInput + linkLast).then(r => {
          console.log(r.data.results);
            array.push(r.data.results);
            //console.log(array);
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
        },

      background(film) {
        return `background-image: url("${film.poster_path == null && film.backdrop_path == null ? 'https://picsum.photos/300/200' :
                  film.poster_path == null ? this.poster_prefix + '/w300' + film.backdrop_path 
                  : this.poster_prefix + '/w300' + film.poster_path}")`
      }

    },
}
</script>

<style lang="scss">
@import "../node_modules/bootstrap";
body {
    background-color: grey;
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }
    header {
        background-color: black;
        color: white;

        .logo {
          color: red;
          font-weight: bold;
          font-size: 2rem;
        }
        input {
            border: none;
            padding: 0.5rem;
            border-radius: 1rem;
            margin-right: 0.5rem;
            font-size: 1.5rem;
        }
        button {
            font-size: 1.5rem;
            padding: 0.5rem 2rem;
            border-radius: 1rem;
            border: none;
        }
    }

     h2 {
            font-size: 3rem;
          color: black;
          font-weight: bold;
      }
    .film {
      padding: 1rem 0.5rem;
      height: 600px;
      position: relative;
      font-size: 1.3rem;
      transition: 0.5s;
      
      .card {
        background-size: cover;
        background-position: top;
        background-repeat: no-repeat;
        height: 100%;
        border-radius: 2rem;
        box-shadow: 5px 5px 5px black;
        transition: 0.5s;
      }

      .description {
          display: none;

          i{
            color: orange;
          }
          transition: 0.5s;
          .lingua {
              display: flex;
              align-items: center;
          }
      }
      
      &:hover .description {
            display: block;
            color: white;
            position: absolute;
            top: 0;
            left: 0;
            padding: 1.5rem 1rem;
      }
      &:hover .card {
        filter: brightness(0.1);
      }

    }
}
</style>
