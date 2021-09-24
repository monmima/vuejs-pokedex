

<template>
  <div class="about">
    <img alt="Vue logo" src="../assets/logo.png">
    <h1>This is the Home page</h1>
    <p>{{ this.$route.params.id }}</p>
    <p>{{ this.$route.params.curOffSet }}</p>
    
    <hr>

    <div>
      <button @click="removeOffset">(===</button>
      <button @click="addOffset">===)</button>
    </div>


    <ul v-if="response.length > 0">
      <li v-for="(item, index) in response" :key="`${item.title}${index}`">
          <router-link :to="`/about/${item.url.split('/')[6]}`" target="_blank" :title="item.name">{{ item.name }}</router-link>
      </li>
    </ul>
    <ul v-else>
      Loading...
    </ul>


  </div>
</template>

<script>


    export default ({
        data() {
            return {
                response: [],
                offset: 0,
                responseOne: []
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

            fetch(`https://pokeapi.co/api/v2/pokemon?limit=20&offset=${curOffset}`)
                .then(res => res.json())

                .then(data => this.response = data.results) // pass the data to the response variable

                // .then(console.log("test"))
                // .then(console.log("other test"))

                .then(data => console.log(data)) // print to the console

            .catch(function (err) {
                console.log('ERROR CATCHED - ' + err);
            });
          },

          fetchOne(id) {

            fetch(`https://pokeapi.co/api/v2/pokemon/${id}/`)
                .then(res => res.json())

                .then(data => this.responseOne = data) // pass the data to the response variable

                // .then(console.log("test"))
                // .then(console.log("other test"))

                .then(data => console.log(data)) // print to the console

            .catch(function (err) {
                console.log('ERROR CATCHED - ' + err);
            });

          }


        },

        mounted: function() {
          this.fetchData(this.curOffset);
        }

    });

</script>

<style>
  button {
    margin: 20px;
    padding: 5px;
  }
</style>
