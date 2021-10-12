<template>
    <span class="dropdown">
        <button class="dropbtn btn btn-primary">Dropdown index</button>
        <div class="dropdown-content">

            <a v-for="i in myArr" :title="`View Pokémons ${i} to ${i + 15}`" :href="`/page/${i}`" :key="`index-${i}`">{{ i }} to {{ i + 15 }}</a>

        </div>
    </span> 
</template>

<script>
    export default {
        name: 'Dropdown',
        data() {
          return {
            myArr: []
          }
        },
        methods: {

            feedArrNb() {
                for (let i = 0; i < 890; i += 15) {
                    this. myArr.push(i);
                    // console.log(this.myArr);
                }
            }

        },
        mounted: function() {
            this.feedArrNb();
        }
    }
</script>

<style lang="scss">

    /* The container <div> - needed to position the dropdown content */
    .dropdown {
        position: relative;
        display: inline-block;
    }

    /* Dropdown Content (Hidden by Default) */
    .dropdown-content {
        display: none;
        position: absolute;
        background-color: #f1f1f1;
        min-width: 160px;
        box-shadow: 0px 8px 16px 0px rgba(0,0,0,0.2);
        z-index: 1;

        height: 300px;
        overflow-y: scroll;
    }

    /* Links inside the dropdown */
    .dropdown-content a {
        color: black;
        padding: 12px 16px;
        text-decoration: none;
        display: block;
    }

    /* Change color of dropdown links on hover */
    .dropdown-content a:hover {
        background-color: #ddd;
    }

    /* Show the dropdown menu on hover */
    .dropdown:hover .dropdown-content {
        display: block;
    }

</style>