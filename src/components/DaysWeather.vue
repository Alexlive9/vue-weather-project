<template>
  <div class="app">
    <div class="days-tab text-center">
      <div v-if="loading">Loading...</div>
      <ul v-else class="p-0">
        <li v-for="day in forecast" :key="day.date" class="li_active">
          <div class="py-3"><img :src="day.iconUrl"></div>
          <div class="py-3">{{ getDayName(day.date) }}</div>

          <div class="py-3">{{ day.temperature }}&deg;C</div>
        </li>

      </ul>
    </div>
  </div>
</template>

<script>

import axios from 'axios';
import moment from 'moment';

export default (await import('vue')).defineComponent({
  props: {
    cityname: String,
  },
  data() {
    return {
      forecast: [],
      loading: true,
      iconUrl: null,
    };
  },
  mounted() {
    this.fetchWeatherData();
  },
  methods: {
    async fetchWeatherData() {
      const apiKey = '8331cdef4f10633c84fd856ce65588b0';
      const city = this.cityname;
      const apiUrl = `https://api.openweathermap.org/data/2.5/forecast?q=${city}&units=metric&appid=${apiKey}`;

      await axios.get(apiUrl).then((response) => {
        const forecastData = response.data.list;
        const filteredData = forecastData
          .map((item) => ({
            date: moment(item.dt_txt.split(' ')[0]),
            temperature: Math.round(item.main.temp),
            description: item.weather[0].description,
            iconUrl: `https://api.openweathermap.org/img/w/${item.weather[0].icon}.png`,
          }))
          .reduce((acc, item) => {
            if (!acc.some((day) => day.date.isSame(item.date, 'day'))) {
              acc.push(item);
            }
            return acc;
          }, [])
          .slice(1, 5);
        this.date = moment().format('DD-MMMM-YYYY');
        this.time = moment().format('HH:mm:ss');
        this.forecast = filteredData;
        this.loading = false;
      }).catch((error) => {
        console.error('Error fetching weather data:', error);
        this.loading = false;
      });
    },
    getDayName(date) {
      return date.format('ddd');
    },
  },
});

</script>

<style>
.days-tab {
  width: 90%;
  box-shadow: 0 4px 8px 0 rgb(121, 121, 199), 0 4px 8px 0 rgb(121, 121, 199);
  border-radius: 20px;
  margin: auto;
}

.loading {
  color: rgb(37, 41, 43);
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
  font-size: 1vm;
  line-height: 1.2;


}

span {
  display: block;
  margin-bottom: 5px;
  font: 100% sans-serif;
  height: 35px;

}

.li_active {
  background: rgb(89, 17, 148);
  color: rgb(183, 112, 238);
  border-radius: 10px;
  margin: 0.5rem;
  color: rgb(232, 218, 241);
  font-weight: 600;

}

.li_active:hover {
  transform: scale(1.2);
  transition: transform 0.1s ease;

}

.li_active_temp {
  display: inline-block;
  background-color: rgb(178, 192, 221);
  color: blueviolet;
  transition: background-color 0.5s;
  border-radius: 10px;

}
</style>





