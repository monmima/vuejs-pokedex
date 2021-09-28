

<template>
  <div class="about">


    <h1>Welcome to my super VueJS Pokédex</h1>
    <p>{{ this.$route.params.id }}</p>
    <p>{{ this.$route.params.curOffSet }}</p>
    
    <hr>

    <div>
      <!-- {{ arrPokemons }} -->
      <form action="#" @submit.prevent="handleSubmit()" >
        <input type="text" placeholder="Search a Pokémon..." focus>
        <button class="btn btn-primary" type="submit">Submit</button>
      </form>

    </div>

    <div class="pokegrid" v-if="arrPokemons.length > 0">
      <!-- <li v-for="(item, index) in response" :key="`${item.title}${index}`">
          <router-link :to="`/about/${item.url.split('/')[6]}`" target="_blank" :title="item.name">{{ item.name }}</router-link>
      </li> -->

        <figure v-for="(item, index) in arrPokemons" :key="`${item.name}${index}`">

          <!-- <img :src="`${item.sprites.front_default}`" :alt="`Image of ${item.name}`">
          <router-link :to="`/about/${item.id}`" target="_blank" :title="item.name.toUpperCase()">{{ item.name.toUpperCase() }}</router-link> -->


          <div class="flip-card">

            <div class="flip-card-inner">
              <div class="flip-card-front">
                <img :src="`${item.sprites.front_default}`" :alt="`Image of ${item.name}`">
                <p>
                  <router-link :to="`/about/${item.id}`" target="_blank" :title="item.name.toUpperCase()">{{ item.name.toUpperCase() }}</router-link>
                </p>
              </div>

              <div class="flip-card-back">
                <h1>John Doe</h1>
                <p>Architect & Engineer</p>
                <p>
                  <router-link :to="`/about/${item.id}`" target="_blank" :title="item.name.toUpperCase()">{{ item.name.toUpperCase() }}</router-link>
                </p>
              </div>
            </div>

          </div> 

        </figure>



    </div>

    <div v-else>
      Loading...
    </div>

    <div>
      <button class="btn btn-primary" @click="addOffset">Show me more Pokémons</button>
    </div>


  </div>
</template>

<script>


    export default ({
        data() {
            return {
                response: [],
                offset: 0,
                arrPokemons: []
            }
        },
        methods: {

          removeOffset() {
            console.log(this.offset -= 20);
            this.fetchData(this.offset);

          },

          addOffset() {
            console.log(this.offset += 20);
            this.fetchData(this.offset);
          },

          handleSubmit() {
            const INPUT = document.querySelector("input").value;

            console.log(INPUT);
            window.location.replace(`/about/${INPUT}`);
          },


          fetchData(curOffset) {
            console.log(curOffset);
            
            for (let i = curOffset + 1; i < curOffset + 19; i++) {
              
              fetch(`https://pokeapi.co/api/v2/pokemon/` + i)
                  .then(res => res.json())

                  .then(data => this.response = data) // pass the data to the response variable

                  .then(data => {
                    this.arrPokemons.push(data);
                  })

              .catch(function (err) {
                  console.log(`ERROR CATCHED on line ${i}: ${err}`);
              });

            } // end of for loop

            console.log(this.arrPokemons);

          } // end of fetch data

        },

        mounted: function() {
          this.fetchData(this.offset);
        }

    });

</script>

<style>
  button {
    margin: 20px;
    padding: 5px;
  }

  figure {
    display: flex;
    align-items: center;
    flex-direction: column;
    justify-content: center;
  }

  figure img {
    max-width: 96px;
  }

  .pokegrid {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
  }

  /* W3Schools stuff - flip cards */

  /* The flip card container - set the width and height to whatever you want. We have added the border property to demonstrate that the flip itself goes out of the box on hover (remove perspective if you don't want the 3D effect */
  .flip-card {
    background-color: transparent;
    width: 300px;
    height: 200px;
    border: 1px solid #f1f1f1;
    perspective: 1000px; /* Remove this if you don't want the 3D effect */
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

  /* Position the front and back side */
  .flip-card-front, .flip-card-back {
    position: absolute;
    width: 100%;
    height: 100%;
    -webkit-backface-visibility: hidden; /* Safari */
    backface-visibility: hidden;
  }

  /* Style the front side (fallback if image is missing) */
  .flip-card-front {
    background-color: #f5f5f5;
    color: black;
  }

  /* Style the back side */
  .flip-card-back {
    background-color: dodgerblue;
    color: white;
    transform: rotateY(180deg);
  } 

</style>
