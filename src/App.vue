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
           console.log(this.listaFilm);
        })
        .catch(function (error) {
           console.error(error);
        });
},

// RICERCA COMPLETA
searchAll(){
   this.searchFilms();
   this.searchSerietv();
}
},

created() {
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

    <h2>Films</h2>
    <div class="results">
      <div v-for="film in listaFilm" :key="film.id" class="film-card">
        <h2>{{ film.title }}</h2>
        <p><strong>Titolo Originale:</strong> {{ film.original_title }}</p>
        <p><strong>Lingua:</strong> {{ film.original_language }}</p>
        <p><strong>Voto:</strong> {{ film.vote_average }}</p>
      </div>
    </div>
      <h2>Serie TV</h2>
    <div class="results">
      <div v-for="serie in listaSerietv" :key="serie.id" class="film-card">
        <h2>{{ serie.name }}</h2>
        <p><strong>Titolo Originale:</strong> {{ serie.original_name }}</p>
        <p><strong>Lingua:</strong> {{ serie.original_language }}</p>
        <p><strong>Voto:</strong> {{ serie.vote_average }}</p>
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
</style>
