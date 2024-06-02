
<script>
  export default {
    props: {
      film: {
        type: Object,
        required: true
      }
    },
    methods: {
      getPosterUrl(path) {
        return path ? `https://image.tmdb.org/t/p/w342${path}` : 'https://via.placeholder.com/342x513?text=No+Image';
      },
      convertToStars(vote) {
        return Math.ceil(vote / 2); 
      }
    }
  };
</script>

<template>
    <div class="card">
      <img :src="getPosterUrl(film.poster_path)" alt="Poster del film">
      <div class="card-overlay">
        <h2>{{ film.title || film.name }}</h2>
        <p><strong>Titolo Originale:</strong> {{ film.original_title || film.original_name }}</p>
        <p><strong>Lingua:</strong> {{ film.original_language }}</p>
        <p><strong>Voto:</strong>
          <span v-for="n in 5" :key="n" class="star">
            <i v-if="n <= convertToStars(film.vote_average)" class="fa fa-star"></i>
            <i v-else class="fa fa-star-o"></i>
          </span>
        </p>
        <p><strong>Overview:</strong> {{ film.overview }}</p>
      </div>
    </div>
</template>
  
<style>
  .card {
  position: relative;
  margin: 10px;
  width: 340px;
  height: 560px;
  background-size: cover;
  background-position: center;
  border-radius: 10px;
  overflow: hidden;
  cursor: pointer;
  transition: transform 0.3s;
}

.card:hover {
  transform: scale(1.05);
}

.card img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.card-overlay {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.7);
  color: #fff;
  padding: 10px;
  opacity: 0;
  transition: opacity 0.3s;
}

.card:hover .card-overlay {
  opacity: 1;
}

.star {
    color: gold;
    font-size: 20px;
    margin-right: 2px;
    margin-left: 5px;
}
</style>