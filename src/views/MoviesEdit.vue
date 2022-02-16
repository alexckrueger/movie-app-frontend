<script>
import axios from "axios";
export default {
  data: function () {
    return {
      movie: {},
      errors: [],
    };
  },
  created: function () {
    axios.get(`/movies/${this.$route.params.id}`).then((response) => {
      console.log("Movie to edit:", response.data);
      this.movie = response.data;
    });
  },
  methods: {
    updateMovie: function () {
      axios
        .patch(`/movies/${this.movie.id}`, this.movie)
        .then((response) => {
          console.log("Movie updated!", response.data), this.$router.push(`/movies/${this.movie.id}`);
        })
        .catch((error) => (this.errors = error.response.data.errors));
    },
    deleteMovie: function () {
      if (confirm("Are you sure???")) {
        axios.delete(`/movies/${this.$route.params.id}`).then((response) => {
          console.log(response.data);
          this.$router.push("/movies");
        });
      }
    },
  },
};
</script>

<template>
  <div class="movies-edit">
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
    <button v-on:click="updateMovie()">Update Movie</button>
    <br />
    <button v-on:click="deleteMovie()">Destroy Movie</button>
  </div>
</template>
