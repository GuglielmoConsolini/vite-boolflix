<script>
import store from './data/store.js';
import axios from 'axios';
export default {
components: {
    
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

  // RICERCA SERIE TV

searchSerietv(){ 
  const options = {
      method: 'GET',
      url: 'https://api.themoviedb.org/3/search/tv',
      params: {query: this.query, include_adult: 'false', language: 'en-US', page: '1'},
      headers: {
        accept: 'application/json',
        Authorization: 'Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiJmOGYyYTNhZjllZDBhYzg4M2RkMWZlMTNiZDVjZjYyYyIsInN1YiI6IjY2NTc0NDA0YTM0ZDFlNzBkNWVmMWUyMyIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.Z-BjUO_PQ4fELYiDy7416HtxhzXgAwbiZJPdIn8o_nI'
      }
    };

  axios
      .request(options)
      .then((response) => {
        this.listaSerietv = response.data.results;
        this.isSearching = true;
        console.log(this.listaSerietv);
      })
      .catch(function (error) {
        console.error(error);
      });
},

//  RICERCA FILMS

searchFilms(){ 
    const options = {
      method: 'GET',
      url: 'https://api.themoviedb.org/3/search/movie',
      params: {query: this.query, include_adult: 'false', language: 'en-US', page: '1'},
      headers: {
       accept: 'application/json',
       Authorization: 'Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiJmOGYyYTNhZjllZDBhYzg4M2RkMWZlMTNiZDVjZjYyYyIsInN1YiI6IjY2NTc0NDA0YTM0ZDFlNzBkNWVmMWUyMyIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.Z-BjUO_PQ4fELYiDy7416HtxhzXgAwbiZJPdIn8o_nI'
      }
    };

    axios
        .request(options)
        .then((response) => {
          this.listaFilm = response.data.results;
          this.isSearching = true;
           console.log(this.listaFilm);
        })
        .catch(function (error) {
           console.error(error);
        });
},

// FUNZIONE PER GENERARE L'INDIRITTO DELLE IMMAGINI
getPosterUrl(path) {
    return path ? `https://image.tmdb.org/t/p/w342${path}` : 'https://via.placeholder.com/342x513?text=No+Image';
},

// RICERCA COMPLETA
searchAll(){
   this.searchFilms();
   this.searchSerietv();
},

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

convertToStars(vote) {
    return Math.ceil(vote / 2); // Arrotonda per eccesso e converte il voto in una scala da 1 a 5
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

  <h1>Boolflix</h1>

<!-- SEARCH BAR -->

  <div class="search-bar position-absolute top-0 end-0">
      <input v-model="query" type="text" placeholder="Cerca un film...">
      <button class="btn btn-danger" @click="searchAll">Cerca</button>
  </div>
  <div v-if="!isSearching">
    <h2>Trending</h2>
    <div class="results">
      <div v-for="elemento in trending" :key="elemento.id" class="film-card">
        <img :src="getPosterUrl(elemento.poster_path)" alt="Poster del film">
        <h2>{{ elemento.title }}</h2>
        <p><strong>Titolo Originale:</strong> {{ elemento.original_title }}</p>
        <p><strong>Lingua:</strong> {{ elemento.original_language }}</p>
        <p><strong>Voto:</strong>
          <span v-for="n in 5" :key="n" class="star">
              <i v-if="n <= convertToStars(elemento.vote_average)" class="fa fa-star"></i>
              <i v-else class="fa fa-star-o"></i>
          </span>
        </p>
      </div>
    </div>
  </div>

  <div v-if="isSearching">
    <h2>Films</h2>
    <div class="results">
      <div v-for="film in listaFilm" :key="film.id" class="film-card">
        <img :src="getPosterUrl(film.poster_path)" alt="Poster del film">
        <h2>{{ film.title }}</h2>
        <p><strong>Titolo Originale:</strong> {{ film.original_title }}</p>
        <p><strong>Lingua:</strong> {{ film.original_language }}</p>
        <p><strong>Voto:</strong>
          <span v-for="n in 5" :key="n" class="star">
              <i v-if="n <= convertToStars(film.vote_average)" class="fa fa-star"></i>
              <i v-else class="fa fa-star-o"></i>
          </span>
        </p>
      </div>
    </div>
      <h2>Serie TV</h2>
    <div class="results">
      <div v-for="serie in listaSerietv" :key="serie.id" class="film-card">
        <img :src="getPosterUrl(serie.poster_path)" alt="Poster del film">
        <h2>{{ serie.name }}</h2>
        <p><strong>Titolo Originale:</strong> {{ serie.original_name }}</p>
        <p><strong>Lingua:</strong> {{ serie.original_language }}</p>
        <p><strong>Voto:</strong>
          <span v-for="n in 5" :key="n" class="star">
              <i v-if="n <= convertToStars(serie.vote_average)" class="fa fa-star"></i>
              <i v-else class="fa fa-star-o"></i>
          </span>
        </p>
      </div>
    </div>
  </div>
</template>

<style scoped>
h1{
  color: red;
}

.search-bar {
  margin-bottom: 20px;
}

.search-bar input {
  padding: 10px;
  font-size: 16px;
  width: 300px;
}

.search-bar button {
  padding: 10px 20px;
  font-size: 16px;
}

.results {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

.film-card {
  border: 1px solid #ddd;
  border-radius: 5px;
  padding: 20px;
  margin: 10px;
  width: 300px;
  text-align: left;
}

.film-card img {
  width: 100%;
  height: auto;
  border-radius: 5px;
  margin-bottom: 10px;
}

.star {
    color: gold;
    font-size: 20px;
    margin-right: 2px;
}
</style>
