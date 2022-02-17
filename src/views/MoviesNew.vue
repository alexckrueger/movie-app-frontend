<script>
import axios from "axios";
export default {
  data: function () {
    return {
      movie: { plot: "" },
      errors: [],
    };
  },
  created: function () {},
  methods: {
    createMovie: function () {
      axios
        .post(`/movies`, this.movie)
        .then((response) => {
          console.log("Movie created!", response.data), this.$router.push(`/movies`);
        })
        .catch((error) => (this.errors = error.response.data.errors));
    },
  },
};
</script>

<template>
  <div class="movies-create">
    <div v-if="errors.length > 0">
      <p v-for="error in errors" v-bind:key="error">{{ error }}</p>
    </div>
    <p>
      Title:
      <input type="text" v-model="movie.title" />
    </p>
    <p>
      Director:
      <input type="text" v-model="movie.director" />
    </p>
    <p>
      Year:
      <input type="text" v-model="movie.year" />
    </p>
    <p>
      Plot:
      <input type="text" v-model="movie.plot" />
    </p>
    <small v-if="movie.plot.length < 141">{{ 140 - movie.plot.length }} characters remaining</small>
    <small id="red" v-if="movie.plot.length > 140">{{ movie.plot.length - 140 }} characters too many!!</small>
    <br />
    <button v-on:click="createMovie()">Update Movie</button>
  </div>
</template>

<style>
#red {
  color: red;
}
</style>
