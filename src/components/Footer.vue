<template>
    <hr>
    
    <footer class="center-text">


      <div>

        <p>This project was made possible thanks to:</p>

        <ul>
          <li>VueJS</li>
          <li>PokéAPI</li>
          <li>Bootstrap</li>
          <li>Heroku</li>
          <li>Github</li>
        </ul>

      </div>

      <p>September 2021</p>

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


