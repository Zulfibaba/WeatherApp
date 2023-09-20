<template>
  <div class="app">
    <div class="container header mt-5">
      <div class="row justify-content-center">
        <div class="col-12 text-center">
          <h1 class="mb-5">Weather App</h1>
        </div>
        <div class="col-12 d-flex justify-content-center">
          <div class="col-md-6">
            <form class="input-group">
              <input type="text" class="form-control rounded" v-model="city" placeholder="Enter City Name">
              <div class="input-group-append">
                <button class="btn btn-primary" type="button" autocomplete="on" @click.prevent="searchWeather">Search <i
                    class="fa-solid fa-magnifying-glass"></i></button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
  <WeatherCard :city="city" v-if="showWeather"></WeatherCard>
</template>

<script>

import WeatherCard from './components/AppWeather.vue';


export default {
  name: 'App',
  components: {
    WeatherCard,

  },
  data() {
    return {
      city: '',
      showWeather: false
    };
  },
  methods: {
    async searchWeather() {
      this.showWeather = false;
      await this.$nextTick();
      this.showWeather = true;
    }
  }
};
</script>



<style>
body {
  background: #121212 !important;
}

.header {
  background: #36454F;
  border-radius: 20px;
  color: white;
  text-align: center;
  font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
  margin-top: 5rem;
  padding-bottom: 2rem;

}

.btn {
  margin-left: 5px;
  background-image: linear-gradient(to right, cyan, magenta);
}

.btn:hover {
  transform: scale(0.9);
  transition: transform 0.1s ease;
}

.input-group {
  margin-bottom: 1rem;
}
</style>

