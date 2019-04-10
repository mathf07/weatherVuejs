<template>
    <div class="actual-weather">
        <div class="ui stackable grid">
            <div class="row">
                <div class="sixteen column center aligned ">
                    <h3><i class="map marker alternate icon"></i>{{weatherLocation()}}</h3>
                </div>
            </div>
            <div class="three column row">
                <div class="column left aligned">
                    <p><i class="sun  icon"></i> {{actualSunrise()}}</p>  
                    <p><i class="moon icon"></i> {{actualSunset()}}</p>
                    <p class="actual-condition-details">
                        <i class="icons">
                            <i class="tint icon"></i>
                            <i class="inverted corner percent icon"></i>
                        </i>
                        {{actualHumidity()}}
                    </p>
                    <p><i class="flag icon"></i>{{actualWind()}}</p>      
                </div>
                <div class="column center aligned">
                    <img :src="actualWeatherIcon()" alt="">
                    <p class="actual-condition">{{actualWeatherCondition()}}</p>
                </div>   
                <div class="column center aligned">
                    <p class="actual-temp"><i class="thermometer half icon"></i>{{actualWeatherTemp()}} <span class="temp-min"> ({{tempMin()}} | {{tempMax()}}) </span></p>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'ActualWeather',
    
    props: {

        datas: Object,
        
    },
    
    methods: {
            actualWeatherIcon() {
                return this.datas.current_condition.icon_big ? this.datas.current_condition.icon_big : ''
            },
            weatherLocation() {
                return this.datas.city_info.name ? this.datas.city_info.name + ', ' + this.datas.city_info.country : ''
            },
            weatherPostalCode() {
                return this.datas.city_info.npa ? this.datas.city_info.npa  : ''
            },
            actualWeatherTemp() {
                return this.datas.current_condition.tmp ? this.datas.current_condition.tmp + '°C' : ''
            },
            actualWeatherCondition() {
                return this.datas.current_condition.condition ? this.datas.current_condition.condition : ''
            },
            tempMin() {
                return this.datas.fcst_day_0 ? this.datas.fcst_day_0.tmin + '°C' : ''
            },
            tempMax() {
                return this.datas.fcst_day_0 ? this.datas.fcst_day_0.tmax + '°C': ''
            },
            actualHumidity() {
                return this.datas.current_condition.condition ? this.datas.current_condition.humidity + '%' : ''
            },
            actualSunrise() {
                return this.datas.city_info.name ? this.datas.city_info.sunrise : ''
            },
            actualSunset() {
                return this.datas.city_info.name ? this.datas.city_info.sunset : ''
            },
            actualWind() {
                return this.datas.current_condition.condition ? this.datas.current_condition.wnd_spd + ' km/h ' + this.windDirToString(this.actualWindDir()) : ''
            },
            actualWindDir() {
                return this.datas.current_condition.condition ? this.datas.current_condition.wnd_dir : ''
            },
            windDirToString(d){
                const words = {
                    N : 'Nord ',
                    S : 'Sud ',
                    E : 'Est ',
                    O : 'Ouest '
                }
                let letters = d.split("")
                let str = ''
                letters.forEach(letter => {
                    str += words[letter]
                })
                return str

            }
           

        }

}
</script>

<style scoped>
    .actual-condition{
        font-size:1.2em;
    }
    .actual-condition-details {
        font-size: 1rem;
        font-weight: 300
    }
    .actual-weather {
        max-width: 60%;
        margin: 50px auto;
    }
     @media screen and (max-width: 640px) {
        .actual-weather {
            max-width: 100%;
        }
    }
</style>



