<template>
    <div class="days-tab text-center">
        <div v-if="loading" class="loading">Loading...</div>
        <ul v-else class="p-0">
            <li v-for="day in forecast" :key="day.date" class="li_active">
                <div class="py-3"><img :src="day.iconUrl"></div>
                <div class="py-3">{{ getDayName(day.date) }}</div>
                <div class="py-3">{{ day.temperature }}&deg;C</div>
            </li>


        </ul>
    </div>
</template>

<script>

import moment from 'moment';
import axios from 'axios';
export default (await import('vue')).defineComponent({
    props: {
        cityname: String,
    },

    data() {
        return {
            forecast: [],
            loading: true,
            iconUrl: null,

        }
    },

    mounted() {
        this.fetchWeatherData()
    },

    methods: {
        async fetchWeatherData() {
            const apiKey = 'fe3460f1f7cfa8a287bd5a2869917cac';
            const city = this.cityname;
            const apiUrl = `https://api.openweathermap.org/data/2.5/forecast?q=${city}&units=matric&appid=${apiKey}`;


            await axios.get(apiUrl).then(Response => {

                const forecastData = Response.data.list
                const filteredData = forecastData.map(item => {
                    return {
                        date: moment(item.dt_txt.split(' ')[0]),
                        temperature: Math.round(item.main.temp - 273.15),
                        description: item.weather[0].description,
                        iconUrl: `https://api.openweathermap.org/img/w/${item.weather[0].icon}.png`
                    }
                }).reduce((acc, item) => {
                    if (!acc.some(day => day.date.isSame(item.date, 'day'))) {
                        acc.push(item);
                    }
                    return acc;
                }, []).slice(1, 5);

                console.log(filteredData, "working")
                this.forecast = filteredData;
                this.loading = false
            }).catch(error => {
                console.error('Error fetching weather data: ', error);
                this.loading = false;
            });
        },

        getDayName(date) {
            return date.format('ddd')
        }
    }

})
</script>

<style>
.days-tab {
    border-radius: 15px;
    width: 90%;
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
}

.loading {
    color: #fff;
}

ul {
    margin: 0;
}

li {
    display: inline-block;
    list-style: none;
    height: 100%;
    width: 21%;
    max-width: 21%;
    font-size: 1vw;
    line-height: 1.2;
}

span {
    display: block;
    margin-bottom: 5px;
    font: 100% sans-serif;
    height: 35px;
}

.li_active {
    background: #253d5c;
    color: #222831;
    border-radius: 10px;
    margin: 0.5rem;
    color: #fff;
    font-weight: 600;
}

.li_active:hover {
    transform: scale(1.2);
    transition: transform 0.1s ease;
}

.li_active_temp {
    display: inline-block;
    background-color: #222831;
    color: #ffffff;
    transition: background-color 0.5s;
    border-radius: 10px;
}
@media screen and (max-width: 450px) {
    li{
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        width: 95%;
        max-width: 95%;
        font-size: 1.5rem;
    }
    .li_active img{
        width: 100px;
    }
}
</style>