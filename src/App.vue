<script>
export default {
  data: function () {
    return {
      isLoggedIn: !!localStorage.jwt,
    };
  },
  watch: {
    $route: function () {
      this.isLoggedIn = !!localStorage.jwt;
    },
  },
};
</script>

<template>
  <div id="nav">
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
      <div class="container-fluid">
        <a class="navbar-brand">Movies</a>
        <button
          class="navbar-toggler"
          type="button"
          data-bs-toggle="collapse"
          data-bs-target="#navbarNavAltMarkup"
          aria-controls="navbarNavAltMarkup"
          aria-expanded="false"
          aria-label="Toggle navigation"
        >
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNavAltMarkup">
          <div class="navbar-nav">
            <router-link class="nav-link active" to="/">Home</router-link>
            <router-link class="nav-link" to="/about">About</router-link>
            <span v-if="isLoggedIn">
              <router-link class="nav-link" to="/logout">Logout</router-link>
            </span>
            <span v-else>
              <router-link class="nav-link" to="/login">Login</router-link>
            </span>
            <router-link class="nav-link" to="/movies">Movies</router-link>
            <span v-if="isLoggedIn">
              <router-link class="nav-link" to="/movies/new">New Movie</router-link>
            </span>
            <span v-else>
              <router-link class="nav-link" to="/signup">Signup</router-link>
            </span>
          </div>
        </div>
      </div>
    </nav>
  </div>
  <router-view />
</template>
