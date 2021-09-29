

<template>
  <div class="about">


    <h1>Welcome to my super <strong>VueJS 3</strong> Pokédex</h1>
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
          

          <img :src="`${item.sprites.front_default}`" :alt="`Image of ${item.name}`">
          <router-link :to="`/about/${item.id}`" target="_blank" :title="item.name.toUpperCase()">{{ item.name.toUpperCase() }}</router-link>


          <div class="flip-card">

            <!-- <div class="flip-card-inner">
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
            </div> -->

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

            // console.log(INPUT);
            if (INPUT.length > 0) {
              window.location.replace(`/about/${INPUT}`);
            } else {
              alert("The input field must not be empty.");
            }

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

<style scoped lang="scss">

  button {
    margin: 20px;
    padding: 5px;
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
  }

  figure img {
    max-width: 96px;
  }

  .pokegrid {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
  }

  /*MEDIA QUERIES*/
  @media screen and (max-width: 759px) {
      body
      {
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
      body
      {
          font-size: 1em;
      }
      .pokegrid {
        display: grid;
        grid-template-columns: 1fr 1fr 1fr;
      }
  }
  @media screen and (min-width: 961px) and (max-width: 1100px) {
      body
      {
          font-size: 1.2em;
      }
      .pokegrid {
        display: grid;
        grid-template-columns: 1fr 1fr 1fr 1fr;
      }
  }
  @media screen and (min-width: 1100px) {
      body
      {
          font-size: 1.3em;
      }
      .pokegrid {
        display: grid;
        grid-template-columns: 1fr 1fr 1fr 1fr 1fr;
      }
  }


</style>
