<script>
import axios from "axios";
export default {
  data: function () {
    return {
      movie: {},
    };
  },
  created: function () {
    axios.get(`/movies/${this.$route.params.id}`).then((response) => {
      console.log("Show Movie:", response.data);
      this.movie = response.data;
    });
  },
  methods: {
    movieEditPage: function () {
      this.$router.push(`/movies/${this.movie.id}/edit`);
    },
  },
};
</script>

<template>
  <div class="movies-show">
    <h2>{{ movie.title }}</h2>
    <p>Directed by {{ movie.director }}</p>
    <p>{{ movie.year }}</p>
    <p>{{ movie.plot }}</p>
    <p>Critics have called this movie:</p>
    <ul v-for="genre in movie.genres" v-bind:key="genre">
      <li>{{ genre.name }}</li>
    </ul>
    <a :href="`https://letmegooglethat.com/?q=${movie.title}`">More Information</a>
    <br />
    <button v-on:click="movieEditPage">Edit Movie</button>
  </div>
</template>
