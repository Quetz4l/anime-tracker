<template id="app">
  <body>
  <main>
    <h1>My Anime Tracker</h1>
    <AnimeList/>
  </main>
  </body>
</template>


<script>
import AnimeList from "@/views/AnimeList";

export default {
  components: {AnimeList}

}
</script>


<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Fira Sans', sans-serif;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;

  &a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}

body {
  background-color: #EEE;

  main {
    margin: 0 auto;
    max-width: 1000px;
    padding: 1.5rem;
  }
}


</style>


<!--  <nav>-->
<!--    <router-link to="/">Home</router-link> |-->
<!--    <router-link to="/about">About</router-link>-->
<!--  </nav>-->
<!--  <router-view/>-->