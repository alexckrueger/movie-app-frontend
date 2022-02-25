<script>
import axios from "axios";
import * as Tone from "tone";
export default {
  data: function () {
    return {
      movies: [],
      titleFilter: "",
      synth: new Tone.Synth().toDestination(),
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
  computed: {
    filteredMovies: function () {
      return this.movies.filter((movie) => {
        return (
          movie.title.toLowerCase().includes(this.titleFilter.toLowerCase()) ||
          movie.director.toLowerCase().includes(this.titleFilter.toLowerCase())
        );
      });
    },
  },
};
</script>

<template>
  <div class="movies-index">
    <div class="container">
      <div class="input-group mb-3 mt-3">
        <span class="input-group-text">Search</span>
        <input type="text" class="form-control" v-model="titleFilter" list="movietitles" />
        <datalist id="movietitles">
          <option v-for="movie in filteredMovies" v-bind:key="movie.id">{{ movie.title }}</option>
        </datalist>
      </div>
      <button class="btn btn-danger disabled mb-3" v-on:click="synth.triggerAttackRelease('C4', '8n')">
        This is supposed to play music :(
      </button>
      <transition-group
        appear
        enter-active-class="animate__animated animate__backInDown"
        leave-active-class="animate__animated animate__backOutUp"
      >
        <div class="row row-cols-1 row-cols-md-2 row-cols-lg-3 g-4">
          <div class="col" v-for="movie in filteredMovies" v-bind:key="movie.id">
            <div class="card" style="width: 18rem">
              <img src="https://www.placecage.com/c/460/300" class="card-img-top" />
              <div class="card-body">
                <h5 class="card-title">{{ movie.title }}</h5>
                <p class="card-text text-truncate">
                  {{ movie.plot }}
                </p>
              </div>
              <ul class="list-group list-group-flush">
                <li class="list-group-item">Directed by {{ movie.director }}</li>
                <li class="list-group-item">{{ movie.year }}</li>
              </ul>
              <div class="card-body">
                <button class="btn btn-success" v-on:click="movieShowPage(movie)">More Info</button>
              </div>
            </div>
          </div>
        </div>
      </transition-group>
    </div>
  </div>
</template>
