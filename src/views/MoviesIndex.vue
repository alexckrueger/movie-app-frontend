<script>
import axios from "axios";
export default {
  data: function () {
    return {
      movies: [],
    };
  },
  created: function () {
    axios.get("/movies").then((response) => {
      console.log("movies Index:", response.data);
      this.movies = response.data;
    });
  },
  methods: {
    movieShowPage: function (movie) {
      this.$router.push(`/movies/${movie.id}`);
    },
  },
};
</script>

<template>
  <div class="movies-index">
    <div v-for="movie in movies" v-bind:key="movie.id">
      <h2>{{ movie.title }}</h2>
      <p>Directed by {{ movie.director }}</p>
      <button v-on:click="movieShowPage(movie)">More Info</button>
    </div>
  </div>
</template>
