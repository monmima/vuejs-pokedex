<template>
  <div class="about">
    <h1>Pokémon Details Page</h1>
    <p>Parameter used for this query: {{ this.$route.params.id }}</p>
    <!-- <p>{{ this.response.id }}</p> -->
    
    <hr>

    <!-- condition to check if response object is empty -->
    <div v-if="Object.entries(response).length > 0" :key="this.$route.params.id">

      <div class="grid-one-pokemon" >

        <div class="picture-box">

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

        </div> <!-- end of .picture-box -->

        <div class="info-box">

          <!-- loop on types in the array -->

          <div class="banner-badge">
            <span v-for="(item, index) in response.types" :key="`${item.name}-${index}`"> {{ item.type.name }}
              <!-- insert comma if item not the last one -->
              <span v-if="index + 1 !== response.types.length">, </span>
            </span>
          </div>

          <!-- description -->
          <div class="poke-description">{{ description }}</div>

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

        </div> <!-- end of .info-box -->

      </div> <!-- end of .grid-one-pokemon -->

      <!-- evolution section -->
      <div class="mt-5">

        <div v-if="arrEvo.length > 0">Next: 
          <div v-for="(item, index) in arrEvo" :key="`${arrEvo[index]}-${index}`">
            <router-link :to="`/about/${arrEvo[index]}`" :title="arrEvo[0].toUpperCase()">{{ arrEvo[index].toUpperCase() }}</router-link> 
          </div>
        </div>
        <div v-else>
          <div>No evolution for this Pokémon</div>
        </div>
      </div>

      <!-- end of evolution section -->

    </div> <!-- end of loop -->

    <!-- response array is empty; Pokémon might be non-existent -->
    <div v-else>
      If a result for your query does not appear here within a few seconds, there was an error or your Pokémon is non-existent.
    </div>

  </div> <!-- end of .about div -->
</template>

<script>

  export default ({
      data() {
        return {
          response: {},
          response2: {},
          response3: {},
          arrEvo: [],
          description: "Loading..."
        }
      },
      methods: {
        handleJSON() {
          // push an evolution to the arrEvo
          try {
            this.arrEvo.push(this.response3.chain.evolves_to[0].species.name);
          }
          catch {
            console.log("CAUGHT ERROR: this.arrEvo.push(this.response3.chain.evolves_to[0].species.name); is non-existent for this Pokémon.");
          }
          
          // push another evolution to arrEvo if it exists
          try {
            this.arrEvo.push(this.response3.chain.evolves_to[0].evolves_to[0].species.name);
          }
          catch {
            console.log("CAUGHT ERROR: this.response3.chain.evolves_to[0].evolves_to[0].species.name is non-existent for this Pokémon.");
          }

          // remove duplicates from arrEvo
          this.arrEvo = [...new Set(this.arrEvo)];
        
          // empty arrEvo if last item (last possible evolution) matches the name of current Pokémon
          if (this.response2.name === this.arrEvo[this.arrEvo.length - 1]) {
            this.arrEvo = [];
          // remove name of current Pokémon from the start of arrEvo so it won't show up in the view
          } else if (this.response2.name === this.arrEvo[0]) {
            this.arrEvo.shift();
          }

          // store data description in an array; the content of the variable is then rendered in the HTML
          // used to avoid a bug in the console even if the data is rendering correctly in the HTML if this.response2.flavor_text_entries[0].flavor_text is used directly; icigo
          // use a filter loop to filter out the first item whose language is English
          // used because language are not stored in a specific order and you might end up with Chinese showing up in the interface because Chinese is at index 0
          const objDesc = this.response2.flavor_text_entries.find((item) => item.language.name === "en");
          this.description = objDesc.flavor_text.replace("\u000c", " ");
          this.description = this.description.replace("POKéMON", "POKÉMON");
          // console.log(this.description);

          // log name to console
          console.log(`Evolution array for ${this.response2.name}:`);
          // log evolution array
          console.log(this.arrEvo);

        },
      async fetchOne() {
          // https://www.pluralsight.com/guides/handling-nested-http-requests-using-the-fetch-api

          this.response = await fetch(`https://pokeapi.co/api/v2/pokemon/${this.$route.params.id}/`).then(response => response.json());
          await console.log("Fetched Pokémon:");
          await console.log(this.response);

          this.response2 = await fetch(`https://pokeapi.co/api/v2/pokemon-species/${this.$route.params.id}/`).then(response => response.json());
          await console.log("Fetched Pokémon species:");
          await console.log(this.response2);

          this.response3 = await fetch(`${this.response2.evolution_chain.url}`).then(response => response.json());
          await console.log("Fetched evolution chain:");
          await console.log(this.response3);

          await this.handleJSON();

        } // end of fetchOne
      }, // end of methods
      mounted: function() {
        this.fetchOne();
      }
  });

</script>

<style scoped lang="scss">

// colors
$background-color: #F8F9FA;

// mixins
@mixin flex-center-center {
  display: flex;
  align-items: center;
  flex-direction: column;
  justify-content: center;
}

a {
  text-decoration: none;
}

/* Flip card rules by W3Schools */

.banner-badge {
  background-color: black;
	color: white;
}

.grid-info-box {
	display: grid;
	grid-template-columns: 1fr 1fr;
	justify-content: center;
}

.grid-one-pokemon {
	display: grid;
	grid-template-columns: 1fr 1fr;
	grid-template-rows: 1fr;
}

.info-box {
	background-color: $background-color;
	// box-shadow: 5px 5px 5px;
	font-weight: 700;
  max-width: 500px;
	padding: 10px 10px 0px 10px;
}

.flip-card {

	background-color: transparent;
  // box-shadow: 5px 5px 5px;
  height: 200px;
	margin-bottom: 20px;
	perspective: 1000px;
  width: 300px;

	&:hover {
		.flip-card-inner {
			transform: rotateY(180deg);
		}
	}

}

.flip-card-back {
  backface-visibility: hidden;
	background-color: $background-color;
  height: 100%;
	position: absolute;
  transform: rotateY(180deg);
	width: 100%;

  @include flex-center-center();
}

.flip-card-front {
  backface-visibility: hidden;
	background-color: $background-color;
  height: 100%;
  position: absolute;
	width: 100%;

  @include flex-center-center();
}

.flip-card-inner {
	height: 100%;
  position: relative;
	text-align: center;
  transform-style: preserve-3d;
	transition: transform 0.8s;
  width: 100%;
}

.picture-box {
  @include flex-center-center();
}

.poke-description {
  background-color: #fff;
  margin: 15px;
  padding: 15px;
}

@media screen and (max-width: 1025px) {
	body {
		font-size: 0.8em;
	}
	.grid-one-pokemon {
    @include flex-center-center();
	}
}

</style>