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
    <div>
      Search by Title or Director:
      <input style="background-color: SlateBlue" type="text" v-model="titleFilter" list="movietitles" />
      <datalist id="movietitles">
        <option v-for="movie in filteredMovies" v-bind:key="movie.id">{{ movie.title }}</option>
      </datalist>
    </div>
    <button style="background-color: DodgerBlue" v-on:click="synth.triggerAttackRelease('C4', '8n')">
      This is supposed to play music :(
    </button>
    <transition-group
      appear
      enter-active-class="animate__animated animate__backInDown"
      leave-active-class="animate__animated animate__backOutUp"
    >
      <div v-for="movie in filteredMovies" v-bind:key="movie.id">
        <h2 style="background-color: crimson">{{ movie.title }}</h2>
        <p>Directed by {{ movie.director }}</p>
        <button style="background-color: chocolate" v-on:click="movieShowPage(movie)">More Info</button>
      </div>
    </transition-group>
  </div>
</template>
