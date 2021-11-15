<template>
  <div id="nav">
    <!-- <router-link to="/">Home</router-link> | -->
    <!-- <router-link to="/about">About</router-link> -->
    <router-link to="/" title="Home Page"><img src="https://upload.wikimedia.org/wikipedia/commons/9/98/International_Pok%C3%A9mon_logo.svg" alt="Logo Pokémon"></router-link>
  </div>

  <router-view :key="$route.fullPath" />

  <Footer />
</template>

<script>
import Footer from './components/Footer.vue'

export default {
  name: 'App',
  components: {
    Footer
  }
}
</script>


<style lang="scss">

  body {
    background-color: lightgray;
  }

  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    min-height: 100vh;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;

    display: flex;
    flex-direction: column;
    justify-content: space-between;
    min-height: 100vh;
  }

  #nav {


    padding: 30px;
    a {
      font-weight: bold;
      color: #2c3e50;
    }
    a.router-link-exact-active {
      color: #42b983;
    }
  }

</style>
