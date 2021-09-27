

<template>
  <div class="about">
    <!-- <img alt="Vue logo" src="../assets/logo.png"> -->
    <img src="https://upload.wikimedia.org/wikipedia/commons/9/98/International_Pok%C3%A9mon_logo.svg" alt="Logo Pokémon">
    <h1>This is the Home page</h1>
    <p>{{ this.$route.params.id }}</p>
    <p>{{ this.$route.params.curOffSet }}</p>
    
    <hr>



    <div>
      <!-- {{ arrPokemons }} -->
    </div>

    <div class="pokegrid" v-if="arrPokemons.length > 0">
      <!-- <li v-for="(item, index) in response" :key="`${item.title}${index}`">
          <router-link :to="`/about/${item.url.split('/')[6]}`" target="_blank" :title="item.name">{{ item.name }}</router-link>
      </li> -->

        <figure v-for="(item, index) in arrPokemons" :key="`${item.name}${index}`">
          <img :src="`${item.sprites.front_default}`" :alt="`Image of ${item.name}`">
          <router-link :to="`/about/${item.id}`" target="_blank" :title="item.name">{{ item.name }}</router-link>
        </figure>

    </div>

    <div v-else>
      Loading...
    </div>

    <div>
      <button @click="addOffset">I want more Pokémons</button>
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
</style>
