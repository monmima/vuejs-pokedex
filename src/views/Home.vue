
<template>
  <div class="about">

    <h1>Welcome to my super <strong>VueJS 3</strong> Pokédex</h1>
    <p>{{ this.$route.params.id }}</p>
    
    <hr>

    <div class="flex-media-queries">
      <!-- {{ arrPokemons }} -->
      <form action="#" @submit.prevent="handleSubmit()" >
        <input type="text" placeholder="Search a Pokémon..." focus>
        <button class="btn btn-primary" type="submit">Submit</button>
      </form>

      <div class="credits">
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

    </div>

    <div>
      <button class="btn btn-primary" @click="removeOffset">(===</button>
      <button class="btn btn-primary" @click="addOffset">===)</button>
    </div>

    <div class="pokegrid" v-if="arrPokemons.length > 0">

      <figure v-for="(item, index) in arrPokemons" :key="`${item.name}${index}`">
        
        <img :src="`${item.sprites.front_default}`" :alt="`Image of ${item.name}`">
        <router-link :to="`/about/${item.id}`" :title="item.name.toUpperCase()">{{ item.name.toUpperCase() }}</router-link>

      </figure>

    </div>

    <div v-else>
      Loading...
    </div>

    <div>
      <button class="btn btn-primary" @click="removeOffset">(===</button>
      <button class="btn btn-primary" @click="addOffset">===)</button>
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

            console.log(this.offset -= 15);
            this.fetchData(this.offset);

          },

          addOffset() {
            console.log(this.offset += 15);
            this.fetchData(this.offset);
          },

          handleSubmit() {
            const INPUT = document.querySelector("input").value;

            // console.log(INPUT);
            if (INPUT.length > 0) {
              window.location.replace(`/about/${INPUT}`);
            } else {
              alert("The input field cannot be empty.");
            }

          },

          fetchData() {
            // empty content of the array before printing anything else to the view
            this.arrPokemons = [];

            console.log(this.offset);
            
            for (let i = this.offset + 1; i < this.offset + 16; i++) {
              
              fetch(`https://pokeapi.co/api/v2/pokemon/` + i)
                  .then(res => res.json())

                  .then(data => this.response = data) // pass the data to the response variable

                  .then(data => {
                    // this.arrPokemons = [];
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

<style scoped lang="scss">

  @mixin grid($fr) {
    display: grid;
    grid-template-columns: $fr;
  }

  button {
    margin: 25px;
  }

  figure {
    background-color: #F8F9FA;
    box-shadow: 5px 5px 5px;
    margin: 50px 20px 0px;
    min-width: 200px;
    padding: 20px;

    display: flex;
    align-items: center;
    flex-direction: column;
    justify-content: center;

    img {
      max-width: 96px;
    }

  }

  input {
    border-radius: 5px;
    height: 40px;
  }

  .credits {
    margin: 5px;
    text-align: left;

    li {
      // list-style-type: '👉 ';
      list-style-image: url("../../src/assets/pokeball-bullet.png");
    }
  }

  .flex-media-queries {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: space-around;
  }

  /*MEDIA QUERIES*/
  @media screen and (max-width: 759px) {
      body {
        font-size: 0.8em;         
      }
      /* NO MORE GRID HERE, ONLY FLEX */
      .pokegrid {
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        justify-content: center;
      }
  }
  @media screen and (min-width: 760px) and (max-width: 960px) {
      body {
          font-size: 1em;
      }
      .pokegrid {
        @include grid(1fr 1fr 1fr);
      }
  }
  @media screen and (min-width: 961px) and (max-width: 1229px) {
      body {
          font-size: 1.2em;
      }
      .pokegrid {
        @include grid(1fr 1fr 1fr 1fr);
      }
  }
  @media screen and (min-width: 1230px) {
      body {
          font-size: 1.3em;
      }
      .pokegrid {
        @include grid(1fr 1fr 1fr 1fr 1fr);
      }
  }


</style>
