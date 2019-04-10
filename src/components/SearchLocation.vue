<template>
    <div class="input-location">
        <div class="ui action fluid input">
            <input @focus="updateLocation($event.target.value)" @input="updateLocation($event.target.value)" @keydown.enter="searchSubmit" id="location" autocomplete="off" placeholder="Ville ou code postal...">
            <button class="ui grey button" :class="{inverted: isDark}" @click="searchSubmit">Go</button>
        </div>
        <div v-show="cities.length > 0" id="cities">
            <div @click="fillInput(city.url)" class="city" v-for="city in cities">{{city.name}}, {{city.npa}} ({{city.country}})</div>
        </div>    
    </div>
</template>

<script>
export default {
    name: 'SearchLocation',

    props: {
            cities: Array,
            isDark: Boolean
        },

    methods: {
        updateLocation(value) {
            this.$emit('input', value)
            document.querySelector('#cities').style.display = "block"
        },
        
        searchSubmit() {
            this.$emit('search-submit')
            
        },
        fillInput(cityUrl) {
            document.querySelector('#cities').style.display = "none"
            this.$parent.loadData(cityUrl)

        }


    }
}
</script>

<style scoped>
    #location {
    position: relative;
    }
    #cities {
        position: absolute;
        z-index: 100;
        background: white;
    }
    .city {
        border-bottom: 0.8px solid rgba(34, 36, 38, 0.15);
        padding: 8px 10px;
        cursor: pointer;
        color: #37474F;
        text-align: left;
    }
    .city:hover {
        background-color: #eff0f1;
    }
    .search {
    font-size: 1rem;
    margin: 0 auto;
    padding: 50px 200px 10px 200px;
    text-align: center;
    }
    @media screen and (max-width: 640px) {
        .search {
            padding: 50px 0px;
        }
    }
</style>


