<template>
  <div class="about">
    <h1>This is an about page</h1>
    <p>{{ this.$route.params.id }}</p>
    
    <hr>

    <div>

      <p>name: {{ response.name }}</p>
      <img :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${ this.$route.params.id }.png`" alt="">

    </div>


  </div>
</template>

  <script>

      export default ({
          data() {
              return {
                  response: []
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

