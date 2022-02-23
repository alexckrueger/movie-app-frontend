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
    <form>
      <div v-if="errors.length > 0">
        <p v-for="error in errors" v-bind:key="error">{{ error }}</p>
      </div>
      <div>
        <label for="title">Title:</label>
        <input id="title" style="background-color: Tomato" type="text" v-model="movie.title" />
      </div>
      <div>
        <label for="director">Director:</label>
        <input id="director" style="background-color: Tomato" type="text" v-model="movie.director" />
      </div>
      <div>
        <label for="year">Year:</label>
        <input id="year" style="background-color: Tomato" type="text" v-model="movie.year" />
      </div>
      <div>
        <label for="plot">Plot:</label>
        <input id="plot" style="background-color: Tomato" type="text" v-model="movie.plot" />
      </div>
      <small v-if="movie.plot.length < 141">{{ 140 - movie.plot.length }} characters remaining</small>
      <small id="red" v-if="movie.plot.length > 140">{{ movie.plot.length - 140 }} characters too many!!</small>
      <br />
      <button style="background-color: Violet" v-on:click="createMovie()">Update Movie</button>
    </form>
  </div>
</template>

<style>
#red {
  color: red;
}
</style>
