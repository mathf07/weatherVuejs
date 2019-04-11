<template>
    <div class="previsions">
        <div class="ui five column stackable grid internally celled">
            <div class="column " v-for="n in range(0,4)" >
                <div class="ui one column grid prevision-col">
                    <div class="column center aligned">
                        <h4 class="previsions-day">{{stringDay(n)}}</h4>
                    </div>
                    <div class="two column row">
                        <div class="column center aligned">
                            <img :src="weatherDayIcon(n)">
                        </div>
                        <div class="column middle aligned center aligned">
                            <div class="previsions-temp-min-max">
                                <p>{{tempMin(n)}}</p>
                                <div class="ui divider"></div>
                                <p>{{tempMax(n)}}</p>
                            </div>
                        </div>
                    </div>
                    <div class="column center aligned">
                        <span>{{dayCondition(n)}}</span>
                    </div>
                    <div class="column center aligned">
                        <button class="ui icon button small grey" :class="{inverted: isDark}" @click="showDetails(n)">Voir le détail <i class="plus circle icon"></i></button>
                    </div>
                </div>
                

                <modal :date="date(n)" :day="stringDay(n)" :id="'myModal'+n" :is-dark="isDark" ref="modal">
                    
                    <div :data-details="n" class="previsions-details"  >
                        <div class="ui stackable grid details-column" >
                            <div class="three column row" v-for="(value, key) in datas['fcst_day_'+ n]['hourly_data']">  
                                <div class="column middle aligned">
                                    <i class="icon clock outline"></i> {{key}}
                                </div>
                                <div class="column">
                                    <img :src="value.ICON">
                                    <p>{{value.CONDITION}}</p>
                                </div>
                                <div class="column middle aligned">
                                    <p>
                                        <i class="thermometer half icon"></i> {{value.TMP2m}} °C
                                    </p>
                                    <p v-if="parseFloat(value.APCPsfc) > 0">
                                        <i class="tint icon"></i> {{value.APCPsfc}} mm
                                    </p>
                                </div>
                                <hr>
                            </div>   
                        </div>
                    </div>
                    
                </modal>

            </div>
        </div>
    </div>
</template>

<script>
import Modal from "./Modal.vue";


export default {
    name: 'Previsions',
    
    props: {

            datas: Object,
            isDark: Boolean

        },
    
    components: {Modal},

    methods: {
        range(min,max) {
            let array = [],
            j = 0;
            for(var i = min; i <= max; i++){
                array[j] = i;
                j++;
            }
            return array;
        },
        weatherDayIcon(n) {
            let day = 'fcst_day_'+ n
            return this.datas.fcst_day_0 ? this.datas[day]['icon_big'] : ''
        },

        stringDay(n) {
            let day = 'fcst_day_'+ n
            if(n === 0) {
                return `Aujourd'hui`
            }
            else if (n === 1) {
                return 'Demain'
            }
            return this.datas.fcst_day_0 ? this.datas[day]['day_long'] : ''
        },

        dayCondition(n) {
            let day = 'fcst_day_'+ n
            return this.datas.fcst_day_0 ? this.datas[day]['condition'] : ''
        },

        tempMin(n) {
            let day = 'fcst_day_'+ n
            return this.datas.fcst_day_0 ?  this.datas[day]['tmin'] + '°C' : ''
        },

        tempMax(n) {
            let day = 'fcst_day_'+ n
            return this.datas.fcst_day_0 ? this.datas[day]['tmax'] + '°C' : ''
        },
        date(n) {
            let day = 'fcst_day_'+ n
            return this.datas.fcst_day_0 ? this.datas[day]['date'] : ''
        },

        

        showDetails(n) {
            this.$refs.modal[n].openModal()
            
        }

    }


}
</script>

<style scoped>
    .previsions {
        margin-top: 60px;
    }
</style>


