<template>
  <div id="app">
    <nav class="navbar is-primary" role="navigation" aria-label="main navigation">
      <div class="container">
        <div class="navbar-brand">
          <router-link to="/" class="navbar-item">
            <span class="icon">
              <i class="fas fa-bullseye fa-3x"></i>
            </span>
          </router-link>

          <span role="button" class="navbar-burger burger" aria-label="menu" aria-expanded="false" data-target="techTinderNav" @click="toggleNavigation">
            <span aria-hidden="true" v-if="!loggedIn"></span>
            <span aria-hidden="true" v-if="!loggedIn"></span>
            <span aria-hidden="true" v-if="!loggedIn"></span>
            <i class="fa fa-user" v-if="loggedIn"></i>
          </span>
        </div>

        <div id="techTinderNav" class="navbar-menu" :class="{ 'is-active' : showNavigation }" >
          <div class="navbar-start">
            <router-link to="/vote" class="navbar-item" active-class="is-active">Vote</router-link>
            <router-link to="/suggest" class="navbar-item" active-class="is-active">Suggest</router-link>
            <router-link to="/stats" class="navbar-item" active-class="is-active">Stats</router-link>
            <router-link to="/radar" class="navbar-item" active-class="is-active">Radar</router-link>
            <router-link to="/about" class="navbar-item" active-class="is-active">About</router-link>
          </div>
          <!-- login -->
          <div class="navbar-end">
            <form class="navbar-item" v-on:submit.prevent @submit="login" v-if="!loggedIn">
              <div class="field has-addons has-addons-centered">
                <p class="control">
                  <input class="input" type="text" placeholder="username" v-model="username" />
                </p>
                <p class="control">
                  <button type="submit" class="button is-primary" :disabled="!username || username.length < 4">
                    Login
                  </button>
                </p>
              </div>
            </form>
            <p class="navbar-item" v-if="loggedIn">
              <span style="margin-right: 10px">Welcome {{username}}!</span>
              <button class="button is-light" @click="logout">
                <span class="icon"><i class="fas fa-sign-out-alt" /></span>
                <span>Logout</span>
              </button>
            </p>
          </div>
        </div>
      </div>
    </nav>
    <router-view/>
  </div>
</template>

<script>
export default {
  data() {
    const aUsername = localStorage.getItem("username");
    const isLoggedIn = aUsername !== null;

    return {
      showNavigation: false,
      loggedIn: isLoggedIn,
      username: aUsername
    };
  },
  watch: {
    $route: function() {
      this.showNavigation = false;
    }
  },
  methods: {
    toggleNavigation: function() {
      this.showNavigation = !this.showNavigation;
    },

    login: function() {
      this.loggedIn = true;
      localStorage.setItem("username", this.username);

      this.$router.go(); // hack
    },

    logout: function() {
      this.username = null;
      this.loggedIn = false;

      localStorage.removeItem("username");

      this.$router.go(); // hack
    }
  }
};
</script>

<style>
.burger .fa-user {
  position: absolute;
  margin: 0 auto;
  top: 15px;
  left: 18px;
}
</style>

