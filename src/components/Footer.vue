<template>
    <hr>
    
    <footer class="center-text">


      <div>

        <p>This project was made possible thanks to:</p>

        <ul>
          <li>VueJS 3</li>
          <li>PokéAPI</li>
          <li>Bootstrap</li>
          <li>SCSS</li>
          <li>Heroku (<a href="https://vuejs-pokedex.herokuapp.com/" title="Link to the project on Heroku">project on Heroku</a>)</li>
          <li>Github (<a href="https://github.com/monmima/vuejs-pokedex" title="Link to the project on Github">project on Github</a>)</li>
          <li>W3Schools</li>
        </ul>

      </div>

      <p><time datetime="2021-09">September 2021</time></p>

    </footer>

</template>

<script>
  export default {
    name: 'Footer'
  }
</script>

<style scoped>

  footer {
    font-size: 0.8em;
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

</style>


