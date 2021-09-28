<template>
  <div class="about">
    <h1>This is an about page</h1>
    <p>Parameter used for this query: {{ this.$route.params.id }}</p>
    <!-- <p>{{ this.response.id }}</p> -->
    
    <hr>

    <!-- condition to check if response object is empty -->
    <div class="flex-one-pokemon" v-if="Object.entries(response).length > 0">


      <div class="picture-box">
        <p><strong>{{ response.name.toUpperCase() }}</strong></p>
        <!-- <img :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${ this.$route.params.id }.png`" alt=""> -->
        <img :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${ this.response.id }.png`" alt="">
        <img :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/back/${ this.response.id }.png`" alt="">
      </div>

      <div class="info-box">
        <div>BASE EXPERIENCE: {{ response.base_experience }}</div>
        <div>{{ response.stats[0].stat.name.toUpperCase() }}: {{ response.stats[0].base_stat }}</div>
        <div>{{ response.stats[1].stat.name.toUpperCase() }}: {{ response.stats[1].base_stat }}</div>
        <div>{{ response.stats[2].stat.name.toUpperCase() }}: {{ response.stats[2].base_stat }}</div>
        <div>{{ response.stats[3].stat.name.toUpperCase() }}: {{ response.stats[3].base_stat }}</div>
        <div>{{ response.stats[4].stat.name.toUpperCase() }}: {{ response.stats[4].base_stat }}</div>
        <div>{{ response.stats[5].stat.name.toUpperCase() }}: {{ response.stats[5].base_stat }}</div>

      </div>

    </div>

    <!-- if response is empty, that means something is wrong; the Pokémon might be non-existant (typing random stuff in the search form will yield this result) -->
    <div v-else>
      If a result for your query does not appear here after a few seconds, there was an error or your Pokémon is non-existent.
    </div>


  </div>
</template>

<script>

  export default ({
      data() {
          return {
              response: {},
              id: 0
          }
      },
      mounted: function() {

          fetch(`https://pokeapi.co/api/v2/pokemon/${this.$route.params.id}/`)
            .then(res => res.json())

            .then(data => this.response = data) // pass the data to the response variable

            // .then(console.log("test"))
            // .then(console.log("other test"))

            .then(data => console.log(data)) // print to the console

          .catch(function (err) {
              console.log('ERROR CATCHED - ' + err);
          });

      }
  });


</script>

<style scoped>
  .flex-one-pokemon {
    display: grid;

    grid-template-columns: 1fr 1fr;
  }
</style>