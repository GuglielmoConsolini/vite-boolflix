<script>
import store from './data/store.js';
import axios from 'axios';
import AppHeader from './components/AppHeader.vue';
import MovieCard from './components/MovieCard.vue';
import SerietvCard from './components/SerietvCard.vue';
export default {
components: {
    AppHeader,
    MovieCard,
    SerietvCard,
  },
data() {
    return {
      store,
      listaFilm: [],
      listaSerietv: [], 
      trending: [],
      query: '',
      path: '',
      isSearching: false,
    }
},  
  
methods: {

handleSearchResults(listaFilm, listaSerietv) {
      this.listaFilm = listaFilm;
      this.listaSerietv = listaSerietv;
      this.isSearching = true;
},

// FUNZIONE PER GENERARE L'INDIRITTO DELLE IMMAGINI
getPosterUrl(path) {
    return path ? `https://image.tmdb.org/t/p/w342${path}` : 'https://via.placeholder.com/342x513?text=No+Image';
},

// FUNZIONE PER I FILM TRENDING
trendingAll() {
    const options = {
      method: 'GET',
      url: 'https://api.themoviedb.org/3/trending/all/day',
      params: {language: 'en-US'},
      headers: {
        accept: 'application/json',
        Authorization: 'Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiJmOGYyYTNhZjllZDBhYzg4M2RkMWZlMTNiZDVjZjYyYyIsInN1YiI6IjY2NTc0NDA0YTM0ZDFlNzBkNWVmMWUyMyIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.Z-BjUO_PQ4fELYiDy7416HtxhzXgAwbiZJPdIn8o_nI'
      }
    };

    axios
      .request(options)
      .then((response) => {
        this.trending = response.data.results;
        this.isSearching = false;
        console.log(this.trending);
      })
      .catch(function (error) {
        console.error(error);
       });
},

// FUNZIONE PER ARROTONDARE IL VOTO
convertToStars(vote) {
    return Math.ceil(vote / 2); 
},

},

created() {
  this.trendingAll();
},

mounted() {
  
}

}

</script>

<template>

  <!-- ***COMPONENTE HEADER*** -->
  <AppHeader @search-results="handleSearchResults" />

<!-- ****SEZIONE FILMS DI TENDENZA****   -->
  <div v-if="!isSearching">
    <h2 class="text-center mt-3 text-danger bg-black">Trending</h2>
    <div class="results">
        <div v-for="elemento in trending" :key="elemento.id" class="card">
          <img :src="getPosterUrl(elemento.poster_path)" alt="Poster del film">
          <div class="card-overlay">
            <h2>{{ elemento.title || elemento.name }}</h2>
            <p><strong>Titolo Originale:</strong> {{ elemento.original_title || elemento.original_name }}</p>
            <p><strong>Lingua:</strong> {{ elemento.original_language }}</p>
            <p><strong>Voto:</strong>
              <span v-for="n in 5" :key="n" class="star">
                <i v-if="n <= convertToStars(elemento.vote_average)" class="fa fa-star"></i>
                <i v-else class="fa fa-star-o"></i>
              </span>
            </p>
            <p><strong>Overview:</strong> {{ elemento.overview }}</p>
          </div>
        </div>
    </div>
  </div>
  
<!-- ***COMPONENTE PER I FILMS*** -->
  <div v-if="isSearching">
    <h2 class="text-center text-danger mt-3 bg-black">Film</h2>
      <div class="results">
        <MovieCard v-for="film in listaFilm" :key="film.id" :film="film" />
      </div>
  </div>
      
<!-- *****COMPONENTE SERIE TV***** -->
  <div v-if="isSearching">
    <h2 class="text-center text-danger mt-3 bg-black">Serie TV</h2>
    <div class="results">
        <SerietvCard v-for="serie in listaSerietv" :key="serie.id" :serie="serie" />
    </div>
  </div>
</template>

<style scoped>
.results {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}
</style>
