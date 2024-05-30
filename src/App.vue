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
      
    }
  },  
  
  methods: {

   films(){ 
    const options = {
    method: 'GET',
    url: 'https://api.themoviedb.org/3/search/movie',
    params: {query: 'wars', include_adult: 'false', language: 'en-US', page: '1'},
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
 }
  },

  created() {

    this.films();

  },

  mounted() {
    
  }
}
</script>

<template>
  
</template>

<style scoped>

</style>
