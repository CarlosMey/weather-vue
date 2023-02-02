<template>
  <div class="home"> 
    <PageLoader />
    <div class="selected"> 
        <label for="countries" class="labeled block mb-2 text-sm font-medium text-gray-900 dark:text-gray-400">Select an option</label>
        <select v-model="city" @change="getData" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500">
          <option disabled value="">Selecione Ciudad</option>
          <option>Lima</option>
          <option>Trujillo</option>
          <option>Chimbote</option>
          <option>Arequipa</option>
          <option>Tacna</option>
          <option>Ica</option>
          <option>Cusco</option>
        </select>

      </div>
    <div class="container">
      
      <div class="container__upper-data">
        <img v-if="weatherData.main?.temp > 15" src="@/assets/morning.svg" alt=""/>
        <img v-if="weatherData.main?.temp <= 15" src="@/assets/night.svg" alt=""/>
        <div class="container__weather-data">
          <div class="location">
            {{weatherData.name}}
          </div>
          <div class="temperature">
            {{weatherData.main?.temp}}°C
          </div>
        </div>
      </div>
      <div class="container__lower-data">
        <div class="more-info-label">
          More Information
        </div>
        <div class="more-info-container">
          <div class="info-block">
            <div class="info-block-label">
              <img src="@/assets/cold.png" alt="">
              <span>Min</span>
            </div>
            <div class="info-block-value">
              {{weatherData.main?.temp_min}}
            </div>
          </div>
          <div class="info-block">
            <div class="info-block-label">
              <img src="@/assets/hot.png" alt="">
              <span>Max</span>
            </div>
            <div class="info-block-value">
              {{weatherData.main?.temp_max}}
            </div>
          </div>
          <div class="info-block">
            <div class="info-block-label">
              <img src="@/assets/humidity.png" alt="">
              <span>humidity</span>
            </div>
            <div class="info-block-value">
              {{weatherData.main?.humidity}}%
            </div>
          </div>
          <div class="info-block">
            <div class="info-block-label">
              <img src="@/assets/wind.png" alt="">
              <span>Wind</span>
            </div>
            <div class="info-block-value">
              {{weatherData.wind?.speed}}km/h
            </div>
          </div>
        </div>
      </div>
    </div> 
  </div>
</template>

<script>

import 'vue-loading-overlay/dist/vue-loading.css';
import PageLoader from '@/components/PageLoader.vue';

export default {
    name: "HomeView",
    components: { PageLoader },
    data() {
        return {
            metric: "metric",
            cityName:"Lima",
            language: "es",
            apiKey: "37dd064bdf958090f6669e30d963019c",
            weatherData: {},
            city: "",
            cityName: [
                "Lima",
                "Trujillo",
                "Chimbote",
                "Arequipa",
                "Tacna",
                "Ica",
                "Cusco",
            ],
            isLoading: false,
            fullPage: true,
        };
    },
    methods: {
        async getData() {
            const res = await fetch(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=${this.apiKey}&units=${this.metric}&lang=${this.language}`);
            const data = await res.json();
            this.weatherData = data;
            console.log(data);
        },
        onChange(event) {
            console.log(event.target.value);
        },
        async getCountry() {
            const ress = await fetch(`https://restcountries.com/v3.1/all`);
            const dataa = await ress.json();
            this.weatherCountry = dataa;
        },

        
    async created() {
        await this.getData();
        await this.getCountry();
    },
  },
  computed: {
        cityComputed() {
            this.onChange();
        },
  }
}
</script>
