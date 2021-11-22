# vue-boolflix

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).

<img :src="film.poster_path == null && film.backdrop_path == null ? 'https://picsum.photos/300/200' :
                  film.poster_path == null ? poster_prefix + '/w300' + film.backdrop_path 
                  : poster_prefix + '/w300' + film.poster_path " 
                  alt="poster"
                  >
 <i v-for="filledStar in voto(film) " :key="filledStar" class="fas fa-star"></i> 
<i v-for="emptyStar in 5 - voto(film)" :key="emptyStar" class="far fa-star"></i>