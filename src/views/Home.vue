<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Movie-App!",
      movies: [],
      newMovie: {},
      movieData: {},
      params: {},
      index: 12,
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
    showMovie: function () {
      this.movieData = {};
      this.index += 1;
      if (this.index === 21) {
        this.index = 13;
      }
      axios.get("http://localhost:3000/movies/" + this.index).then((response) => (this.movieData = response.data));
    },
    updateMovie: function () {
      this.params = {
        title: this.movieData.title,
        director: this.movieData.director,
        year: this.movieData.year,
        plot: this.movieData.plot,
      };
      axios
        .patch("http://localhost:3000/movies/" + this.index, this.params)
        .then((response) => console.log(response.data))
        .catch((error) => console.log(error.response));
    },
    destroyMovie: function () {
      axios.delete("http://localhost:3000/movies/" + this.index).then((response) => console.log(response));
    }
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
    <button v-on:click="showMovie()">More info on a Movie</button>
    <div v-if="movieData">
      <h1>{{ movieData.title }}</h1>
      <p>{{ movieData.director }}</p>
      <p>{{ movieData.year }}</p>
      <p>{{ movieData.plot }}</p>
    </div>
    <div>
      <h3>Edit the above movie:</h3>
      <p>
        Title:
        <input type="text" v-model="movieData.title" />
      </p>
      <p>
        Director:
        <input type="text" v-model="movieData.director" />
      </p>
      <p>
        Year:
        <input type="text" v-model="movieData.year" />
      </p>
      <p>
        Plot:
        <input type="text" v-model="movieData.plot" />
      </p>
      <button v-on:click="updateMovie()">Save Changes</button>
    </div>
    <h1>DO YOU WANT TO DESTROY THE MOVIE?</h1>
    <button v-on:click="destroyMovie()">YES! OBLITERATE!</button>
  </div>
</template>

<style></style>
