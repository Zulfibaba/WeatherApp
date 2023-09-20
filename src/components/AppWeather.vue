<template>
    <div class="weather-card container p-0">
        <div class="weather-container">
            <div class="res-card card main-dev w-100">
                <div class="p-3">
                    <h2 class="mb-1 day">Today</h2>
                    <p class="text-light date mb-0"> {{ date }} </p>
                    <small>{{ time }}</small>
                    <h2 class="place"><i class="fa fa-location"></i>{{ name }} <small>{{ Country }}</small></h2>
                    <div class="temp">
                        <h1 class="weather-temp">{{ temperature }}&deg;</h1>
                        <h2 class="text-light">{{ description }} <img :src="iconUrl"> </h2>
                    </div>
                </div>
            </div>
            <div class="card-2 w-100">
                <table class="m-4">
                    <tbody>
                        <tr>
                            <th>Sea Level</th>
                            <td>{{ sea_level }}</td>
                        </tr>
                        <tr>
                            <th>Humidity</th>
                            <td>{{ humidity }}</td>
                        </tr>
                        <tr>
                            <th>Wind</th>
                            <td>{{ wind }}</td>
                        </tr>
                    </tbody>
                </table>

                <DaysWeather :cityname="cityname"></DaysWeather>

                <div id="div_Form" class="d-flex m-3 justify-content-center">
                    <form action="">
                        <input type="button" value="Change Location" @click="changeLocation"
                            class="btn change-btn btn-primary">
                    </form>
                </div>
            </div>
        </div>
    </div>
</template>
  
<script>
import axios from 'axios';
import DaysWeather from './DaysWeather.vue';

export default (await import('vue')).defineComponent({
    name: 'WeatherCard',
    components: {
        DaysWeather,
    },

    props: {
        city: String,
    },
    data() {
        return {
            cityname: this.city,
            temperature: null,
            description: null,
            iconUrl: null,
            date: null,
            time: null,
            name: null,
            sea_level: null,
            wind: null,
            humidity: null,
            country: null,
            monthNames: ["January", "February", "March", "April", "May", "June",
                "July", "August", "September", "October", "November", "December"]

        }
    },

    methods: {
        changeLocation() {
            window.location.reload()
        }
    },

    async created() {
        const response = await axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=matric&appid=fe3460f1f7cfa8a287bd5a2869917cac`)
        const weatherData = response.data;
        this.temperature = Math.round(weatherData.main.temp - 273.15);
        this.description = weatherData.weather[0].description;
        this.name = weatherData.name;
        this.wind = weatherData.wind.speed;
        this.sea_level = weatherData.main.sea_level;
        this.country = weatherData.sys.country;
        this.humidity = weatherData.main.humidity;


        this.iconUrl = `https://api.openweathermap.org/img/w/${weatherData.weather[0].icon}.png`;
        const d = new Date();
        this.date = d.getDate() + '-' + this.monthNames[d.getMonth()] + '-' + d.getFullYear();
        this.time = d.getHours() + ':' + d.getMinutes() + ':' + d.getSeconds();
        console.log(weatherData);



    }
});
</script>
  
<style scoped>
.weather-container {
    display: flex;
    align-items: center;
    justify-content: center;
}

body {
    background-color: #343d4b;
}

.weather-temp {
    margin: 0;
    font-weight: 700;
    font-size: 4em;
}

h2.mb-1.day {
    font-size: 3rem;
    font-weight: 400;
}

.main-dev {
    border-radius: 20px;
    color: #fff;
    background-image: url("https://media.cnn.com/api/v1/images/stellar/prod/230224150645-01-best-beaches-world-tripadvisor-2023-card.jpg?c=original");
    background-size: cover;
    background-position: center;
    background-blend-mode: overlay;
    background-color: #343d4b;
    background-repeat: no-repeat;
    margin-top: 2%;

}

.temp {
    position: absolute;
    bottom: 0;
}

.main-dev:hover {
    transform: scale(1.1);
    transition: transform 0.5s;
    z-index: 1;
}

.card-2 {
    display: flex;
    flex-direction: column;
    align-items: center;
    background-color: #36454F;
    border-radius: 20px;
    margin-top: 2%;
    margin-left: 3%;

}

.card-details {
    margin-left: 19px;
}

.h1_left {
    position: absolute;
    bottom: 25px;
    left: 16px;
    font-size: 3vw;
    line-height: 1.2;
}

.h2_left {
    position: absolute;
    left: 16px;
    font-size: 2vw;
    line-height: 0.5;
}

.h3_left small {
    font-size: 1rem;
}

table {
    position: relative;
    left: 15px;
    border-collapse: separate;
    border-spacing: 15px;
    width: 85%;
    text-align: left;
    max-width: 600px;
    margin: 0 auto;
}

th,
td {
    font-size: 18px;
    color: #fff;
}

td {
    text-align: right;
}

table,
tr:hover {
    color: red;
}

.change-btn {
    background-image: linear-gradient(to right, cyan, magenta);
}

.change-btn:hover {
    transform: scale(0.9);
    transition: transform 0.1s ease;
}

@media screen and (max-width: 450px) {

    .weather-container {
        margin: 0px 20px;
        flex-direction: column;
    }
    .card-2{
        margin-left: 0;
        margin-top: 20px;
    }
    .res-card {
        height: 320px;
        width: 97vw;
    }
}</style>
  
