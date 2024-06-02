<script>
import axios from 'axios';
import store from '../data/store';
export default {
  props: {
    
  },

  data() {
    return {
      query: '',
      store,
      listaFilm: [],
      listaSerietv: [],
      path: '',
      isSearching: false,
    };
  },
methods: {
  // RICERCA COMBINATA
  searchAll(){
    this.searchFilms();
    this.searchSerietv();
  },

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
        this.$emit('search-results', this.listaFilm, this.listaSerietv);
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
        this.$emit('search-results', this.listaFilm, this.listaSerietv);
      })
        .catch(function (error) {
          console.error(error);
        });
  },

}

};
</script>

<template>
  <header>
    <h1>Boolflix</h1>
    <div class="search-bar">
      <input v-model="query" type="text" placeholder="Cerca un film...">
      <button class="btn btn-danger" @click="searchAll">Cerca</button>
    </div>
  </header>
</template>

<style scoped>
header{
  background-color: black;
  padding: 1rem 1.5rem;
  display: flex;
  justify-content: space-between;
}
h1 {
  color: red;
}

.search-bar {
  margin-bottom: 20px;
}

.search-bar input {
  padding: 5px;
  font-size: 16px;
  width: 300px;
  border-radius: 30px;
}

.search-bar button {
  padding: 5px 10px;
  font-size: 16px;
  border-radius: 30px;
}
</style>