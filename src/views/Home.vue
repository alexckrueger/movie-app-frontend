<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Movie-App!",
      movies: [],
      newMovie: {},
    };
  },
  created: function () {
    this.indexMovies();
  },
  methods: {
    indexMovies: function () {
      axios.get("http://localhost:3000/movies").then((response) => (this.movies = response.data));
    },
    createMovie: function () {
      axios
        .post("http://localhost:3000/movies", this.newMovie)
        .then((response) => this.movies.unshift(response.data))
        .catch((error) => console.log(error.response));
      this.newMovie = {};
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <h2>Create a movie!</h2>
    <p>
      Title
      <input type="text" v-model="newMovie.title" />
    </p>
    <p>
      Director
      <input type="text" v-model="newMovie.director" />
    </p>
    <p>
      Year
      <input type="text" v-model="newMovie.year" />
    </p>
    <p>
      Plot
      <input type="text" v-model="newMovie.plot" />
    </p>
    <button v-on:click="createMovie()">Make Movie!</button>

    <div v-for="movie in movies" v-bind:key="movie.id">
      <h3>Title: {{ movie.title }}</h3>
    </div>
  </div>
</template>

<style></style>
