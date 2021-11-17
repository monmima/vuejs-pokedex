<template>
    
    <footer class="center-text">
      <div class="container">

        <div v-if="this.$route.fullPath !== '/'">
          Parameter used to access current page: "{{ this.$route.params.id }}".
        </div>

        <p>This project was made possible thanks to:</p>

        <div class="credits">
          <ul>
            <li>VueJS 3</li>
            <li>PokéAPI</li>
            <li>Bootstrap</li>
          </ul>

          <ul>
            <li>SCSS</li>
            <li>W3Schools</li>
          </ul>

          <ul>
            <li>Heroku (<a href="https://vuejs-pokedex.herokuapp.com/" title="Link to the project on Heroku">project on Heroku</a>)</li>
            <li>Github (<a href="https://github.com/monmima/vuejs-pokedex" title="Link to the project on Github">project on Github</a>)</li>
          </ul>
        </div>

        <p>
          <time datetime="2021">September and October 2021</time>
        </p>

      </div>

    </footer>

</template>

<script>
  export default {
    name: 'Footer'
  }
</script>

<style scoped lang="scss">

  footer {
    background-color: #385FAA;
    color: white;
    font-size: 0.8em;
    padding-top: 1rem !important;

    a {
      color: #FFCB05;
      &:hover {
        color: white;
      }
    }

    .container p {
      text-align: center;
    }
  }

  li, p {
    text-align: left;
  }

  .center-text {
    display: flex;
    align-content: center;
    align-items: center;
    flex-direction: column;
    justify-content: center;
  }

  .credits {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    width: 100%;

    // background-color: pink;

    margin: 5px;
    text-align: left;

    * {
      // background-color: green;
      width: 200px;
    }    

    li {
      // list-style-type: '👉 ';
      list-style-image: url("../../src/assets/pokeball-bullet.png");
    }
  }

</style>


