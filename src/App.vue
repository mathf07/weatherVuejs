<template>
    <div id="app" :class="{ dark: isDark }">
        <h4 class="toggle-color-mode">
            THEME
            <i class="big icon circle" @click="toggleColorTheme"></i>
        </h4>
        <div class="ui container">
            <div class="search">
                <search-location
                v-model="inputLocation"
                @search-submit="loadData(inputLocation)"
                :cities="filteredCities"
                :is-dark="isDark"
                ></search-location>
                <div class="ui hidden divider"></div>
                <error-message :error="err"></error-message>
            </div>
            <div class="ui divider"></div>
            <div class="weather-content" v-if="!loading && err.length === 0">
                <actual-weather :datas="apiData"></actual-weather>
                <div class="ui divider"></div>
                <previsions :datas="apiData" :is-dark="isDark"></previsions>
            </div>
            <div class="ui active dimmer" v-if="loading">
                <div class="ui text large loader">Chargement ...</div>
            </div>
        </div>
    </div>
</template>

<script>
import ActualWeather from "./components/ActualWeather.vue";
import ErrorMessage from "./components/ErrorMessage.vue";
import Previsions from "./components/Previsions.vue";
import SearchLocation from "./components/SearchLocation.vue";
import Modal from "./components/Modal.vue";


const apiBaseUrl = "https://www.prevision-meteo.ch/services/json/";
const apiCities = "https://cors.io/?https://www.prevision-meteo.ch/services/json/list-cities";

export default {
    name: "app",
    
    data() {
        return {
            apiData: {},
            apiCities: [],
            filteredCities: [],
            inputLocation:'',
            loading: false,
            err: '',
            isDark: false
        }
    },
          
    

    components: {ErrorMessage, SearchLocation, ActualWeather, Previsions},

    
    methods: {
        loadData(city)  {
            this.loading = true
            fetch(apiBaseUrl + city)
            .then((res) => {
                return res.json();
            })
            .then(data => {
                // console.log(data);
                !data.errors ? this.apiData = data : this.err = 'Nous ne trouvons pas cet emplacement, veuillez en saisir un autre'
            })
            .catch(err => {
                console.log(err);
                this.err = `Une erreur s'est produite au chargement des données météo`;
            })
            .finally(() => this.loading = false,
                            this.err = '' );
        },

        filterCities(objProp,regex) {
            this.filteredCities = this.apiCities.filter(element => {
                    let city = element[objProp] != null ? element[objProp] : ''
                    return city.match(regex) !== null
                })
        },

        setFilteredCities() {
            this.filteredCities = [];
            if (this.inputLocation.length >= 3 && this.inputLocation.match(/^[0-9]+$/) != null) {
                let reg = new RegExp('^' + this.inputLocation, 'gi')
                this.filterCities('npa', reg)
            }

            else if (this.inputLocation.length >= 3 && this.inputLocation.match(/[A-Z]+$/i) != null) {
                let reg = new RegExp('^' + this.inputLocation, 'gi')
                this.filterCities('name', reg)
                    

                
            }
            
            // console.log(this.filteredCities)
        },
        toggleColorTheme() {
            this.isDark = !this.isDark
        }

        
        
    },

    watch: {
        inputLocation: function () {
            this.setFilteredCities()
        },
        filteredCities: function() {
            console.log(this.filteredCities)
            if(this.filteredCities.length > 0){
                const citiesDiv = document.querySelector('#cities')
                const input = document.querySelector('#location')
                citiesDiv.style.width = input.offsetWidth + 'px'
                window.onclick = function(event) {
                    if (event.target != citiesDiv && event.target != input ) {
                        citiesDiv.style.display = "none"
                    }
                }  

            }
            if(this.filteredCities.length >= 20) this.filteredCities.length = 20
        }


    },



    created() {
        //Load Cities
        this.loadData('Paris');
        fetch(apiCities)
        .then((res) => {
            return res.json();
        })
        .then(data => {
            this.apiCities = Object.keys(data).map(key => {
                let ar = data[key]
                ar.key = key
                return ar
            });
            //Sort cities
            this.apiCities.sort(function(a, b){
                if(a.name < b.name) { return -1; }
                if(a.name > b.name) { return 1; }
                return 0;
                })  
            })
        .catch(err => {
            console.log(err);
            this.err = 'Une erreur s\'est produite au chargement des villes';
        })
        .finally(() => this.loading = false,
                        this.err = '' );
    }
        

  






};
</script>

<style>
    @import url("https://fonts.googleapis.com/css?family=Nunito:200,300,400,600");

    body,
    h1,
    h2,
    h3,
    h4,
    h5,
    .ui.button {
    font-family: "Nunito", sans-serif;
    }
    body {
        color: #262626;
    }
    #app {
    min-height: 100vh;
    }
    .dark {
        background: #262626;
        color: white;
    }
    .ui.dimmer {
    background-color: rgba(0, 0, 0, 0.6);
    }
    .search {
    font-size: 1rem;
    margin: 0 auto;
    padding: 50px 200px 10px 200px;
    text-align: center;
    }
    @media screen and (max-width: 640px) {
        .search {
            padding: 50px 0 10px 0;
        }
    }

  .toggle-color-mode {
    text-align: right;
    top: 10px;
    right: 10px;
    position: absolute;
    }
    .toggle-color-mode i {
    cursor: pointer;
    }
    .actual-temp {
    font-size: 1.5em;
    }
    .temp-min,
    .temp-max,
    .previsions-temp-min-max {
    font-size: 1rem;
    font-weight: 400;
    }
</style>
