<template>
  <div class="about">
    <h1>Pokémon Details Page</h1>
    <p>Parameter used for this query: {{ this.$route.params.id }}</p>
    <!-- <p>{{ this.response.id }}</p> -->
    
    <hr>

    <!-- condition to check if response object is empty -->
    <div class="grid-one-pokemon" v-if="Object.entries(response).length > 0">


      <div class="picture-box">
        <!-- <p><strong>{{ response.name.toUpperCase() }}</strong></p> -->
        <!-- <img :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${ this.$route.params.id }.png`" alt=""> -->

        <!-- images, no flip card -->
        <!-- <img :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${ this.response.id }.png`" alt="">
        <img :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/back/${ this.response.id }.png`" alt=""> -->

        <!-- images in a flip card -->

        <div class="flip-card">
          <div class="flip-card-inner">
            <div class="flip-card-front">
              <!-- <img src="img_avatar.png" alt="Avatar" style="width:300px;height:300px;"> -->
              <h5>{{ response.name.toUpperCase() }} - FRONT</h5>
              <img :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${ this.response.id }.png`" alt="">
            </div>
            <div class="flip-card-back">
              <h5>{{ response.name.toUpperCase() }} - BACK</h5>
              <img :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/back/${ this.response.id }.png`" alt="">
            </div>
          </div>
        </div> 

      </div>


      <div class="info-box">

        <!-- loop on types in the array -->

        <div class="banner-badge">
          <span v-for="(item, index) in response.types" :key="`${item.name}${index}`"> {{ item.type.name }}
            <!-- add a comma between each type -->
            <span v-if="index + 1 !== response.types.length">, </span>
          </span>
        </div>

        <!-- stats grid -->
        <div class="grid-info-box">
          <!-- <p>{{ response.types }}</p> -->
        
          <p>BASE EXPERIENCE</p>
          <p>{{ response.base_experience }}</p>

          <p>{{ response.stats[0].stat.name.toUpperCase() }}</p>
          <p>{{ response.stats[0].base_stat }}</p>

          <p>{{ response.stats[1].stat.name.toUpperCase() }}</p>
          <p>{{ response.stats[1].base_stat }}</p>

          <p>{{ response.stats[2].stat.name.toUpperCase() }}</p>
          <p>{{ response.stats[2].base_stat }}</p>

          <p>{{ response.stats[3].stat.name.toUpperCase() }}</p>
          <p>{{ response.stats[3].base_stat }}</p>
            
          <p>{{ response.stats[4].stat.name.toUpperCase() }}</p>
          <p>{{ response.stats[4].base_stat }}</p>

          <p>{{ response.stats[5].stat.name.toUpperCase() }}</p>
          <p>{{ response.stats[5].base_stat }}</p>
        </div>


        


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
              response2: {},
              response3: {},
              arrEvo: [],
              id: 0
          }
      },
      methods: {
        findIndexOfPokemon(str) {
          return this.arrEvo === str;
        },
        async fetchOne() {

          // https://www.pluralsight.com/guides/handling-nested-http-requests-using-the-fetch-api

          // log the content of the queries
          this.response = await fetch(`https://pokeapi.co/api/v2/pokemon/${this.$route.params.id}/`).then(response => response.json());
          await console.log(this.response);

          this.response2 = await fetch(`https://pokeapi.co/api/v2/pokemon-species/${this.$route.params.id}/`).then(response => response.json());
          await console.log(this.response2);

          this.response3 = await fetch(`${this.response2.evolution_chain.url}`).then(response => response.json());
          await console.log(this.response3);


          // push the content of queries to the array
          this.arrEvo.push(this.response3.chain.evolves_to[0].species.name);
          this.arrEvo.push(this.response3.chain.evolves_to[0].evolves_to[0].species.name);
          this.arrEvo = [...new Set(this.arrEvo)];

          console.log(this.arrEvo);

          console.log(this.arrEvo.indexOf(this.response2.name));

          const indexPokemonInArrEvo = this.arrEvo.indexOf(this.response2.name)

          if (indexPokemonInArrEvo === -1) {
            console.log(`Next evos: ${this.arrEvo[0]}, ${this.arrEvo[1]}`);
          } else if (indexPokemonInArrEvo === 0) {
              console.log(`Next evo: ${this.arrEvo[1]}`);
          } else {
              console.log(`No evolution!`);
          }

        } // end of fetchOne
      }, // end of methods
      mounted: function() {

        this.fetchOne();

      }
  });


</script>

<style scoped lang="scss">
  .banner-badge {
    color: white;
    background-color: black;
  }

  .grid-one-pokemon {
    display: grid;

    grid-template-columns: 1fr 1fr;
    grid-template-rows: 1fr;
  }

  .info-box {
    background-color: #F8F9FA;
    box-shadow: 5px 5px 5px;
    font-weight: 700;
    padding: 10px 10px 0px 10px;
    max-width: 500px;
  }

  .grid-info-box {
    display: grid;
    grid-template-columns: 1fr 1fr;
    justify-content: center;
  }

  .picture-box {
    display: flex;

    flex-direction: column;
    align-items: center;
  }

  /* W3Schools flip card */

  /* The flip card container - set the width and height to whatever you want. We have added the border property to demonstrate that the flip itself goes out of the box on hover (remove perspective if you don't want the 3D effect */
  .flip-card {
    background-color: transparent;
    width: 300px;
    height: 200px;
    /* border: 1px solid #f1f1f1; */
    margin-bottom: 20px;
    perspective: 1000px; /* Remove this if you don't want the 3D effect */

  }

  .flip-card {
    box-shadow: 5px 5px 5px;
  }

  /* This container is needed to position the front and back side */
  .flip-card-inner {
    position: relative;
    width: 100%;
    height: 100%;
    text-align: center;
    transition: transform 0.8s;
    transform-style: preserve-3d;

  }

  /* Do an horizontal flip when you move the mouse over the flip box container */
  .flip-card:hover .flip-card-inner {
    transform: rotateY(180deg);
  }

  .flip-card-front, .flip-card-back {
    /* Style the front side (fallback if image is missing) */
    background-color: #F8F9FA;
    color: black;
    

    /* Position the front and back side */
    position: absolute;
    width: 100%;
    height: 100%;
    -webkit-backface-visibility: hidden; /* Safari */
    backface-visibility: hidden;

    /* flex */
    display: flex;
    align-items: center;
    flex-direction: column;
    justify-content: center;
    justify-items: center;
  }

  /* Style the back side */
  .flip-card-back {
    transform: rotateY(180deg);
  }

  /* media queries */

  @media screen and (max-width: 1025px) {
      body
      {
          font-size: 0.8em;         
      }
      /* NO MORE GRID HERE, ONLY FLEX */
      .grid-one-pokemon {
          display: flex;

          align-items: center;
          flex-direction: column;
          justify-content: center;
      }
  }
  // @media screen and (min-width: 760px) and (max-width: 960px) {
  //     body
  //     {
  //         font-size: 1em;
  //     }
  //     div, figure, figcaption {
  //         width: 150px;
  //     }
  // }
  // @media screen and (min-width: 961px) and (max-width: 1100px) {
  //     body
  //     {
  //         font-size: 1.2em;
  //     }
  //     div, figure, figcaption {
  //         width: 200px;
  //     }
  // }
  // @media screen and (min-width: 1100px) {
  //     body
  //     {
  //         font-size: 1.3em;
  //     }
  //     div, figure, figcaption {
  //         width: 250px;
  //     }
  // }

</style>