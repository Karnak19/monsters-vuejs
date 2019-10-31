<template>
  <b-navbar>
    <template slot="brand">
      <b-navbar-item tag="router-link" :to="{ path: '/' }">
        <img
          src="https://raw.githubusercontent.com/buefy/buefy/dev/static/img/buefy-logo.png"
          alt="Lightweight UI components for Vue.js based on Bulma"
        />
      </b-navbar-item>
    </template>
    <template slot="start">
      <b-navbar-item tag="router-link" :to="{path: '/'}">Home</b-navbar-item>
      <b-navbar-item tag="router-link" :to="{path: '/about'}">Fight !</b-navbar-item>
    </template>

    <template slot="end">
      <b-navbar-item tag="div">
        <div class="buttons">
          <b-button class="is-danger" @click="removeLS">
            <strong>Sign out</strong>
          </b-button>
        </div>
      </b-navbar-item>
    </template>
  </b-navbar>
</template>

<script>
export default {
  name: "Navbar",
  methods: {
    removeLS: function() {
      localStorage.removeItem("name");
      this.$buefy.toast.open({
        duration: 2000,
        position: "is-top",
        type: "is-success",
        message: "Disconnected successfully"
      });
    }
  }
};
</script>

<style scoped>
a {
  font-family: "Permanent Marker", cursive;
}
a.router-link-exact-active.is-active {
  color: #7957d5;
}
</style>