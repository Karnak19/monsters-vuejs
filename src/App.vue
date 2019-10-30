<template>
  <div id="app">
    <Navbar />
    <router-view />
  </div>
</template>

<script>
import Navbar from "@/components/Navbar.vue";

export default {
  name: "App",
  components: {
    Navbar
  }
};
</script>

<style>
@import url("https://fonts.googleapis.com/css?family=Permanent+Marker|Roboto&display=swap");

#app {
  font-family: "Roboto", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
  font-family: "Permanent Marker", cursive;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a {
  color: #42b983;
}
</style>
