<template>
    <span class="dropdown">
        <button class="dropbtn btn btn-primary">Pagination</button>
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

    .btn-primary {
        background-color: #385FAA;
        border-color: #385FAA;
    }

    /* The container <div> - needed to position the dropdown content */
    .dropdown {
        display: inline-block;
        position: relative;

        /* Show the dropdown menu on hover */
        &:hover {
            .dropdown-content {
                display: block;
            }
        }

    }

    /* Dropdown Content (Hidden by Default) */
    .dropdown-content {
        // box-shadow: 0px 8px 16px 0px rgba(0,0,0,0.2);
        border: 2px solid #c5c1c1;
        display: none;
        background-color: white;
        // background-color: #c5c1c1;
        height: 300px;
        min-width: 160px;
        overflow-y: scroll;
        position: absolute;
        z-index: 1;

        /* Links inside the dropdown */
        a {
            color: black;
            display: block;
            padding: 12px 16px;
            text-decoration: none;
            &:hover {
                background-color: #ddd;
            }
        }
    }

</style>