<template>
  <div class="container p-0">
    <div class="d-flex">
      <div class="card card-5 w-100">
        <table class="m-4">
          <tbody>
            <tr>
              <th>Sea Level</th>
              <td v-if="sea_level > 0">{{ sea_level }}</td>
              <td v-else>Null</td>
            </tr>
            <tr>
              <th>Humidity</th>
              <td>{{ humidity }}</td>
            </tr>
            <tr>
              <th>wind</th>
              <td>{{ wind }}</td>
            </tr>
          </tbody>
        </table>

        <DaysWeather :cityname="cityname"></DaysWeather>
        <div id="div_Form" class="d-flex m-3 justify-content-center">
          <form action="">
            <input type="button" value="Change Location" @click="changeLocation" class="btn change_btn btn-primary">
          </form>
        </div>
      </div>
      <div class="card main-div w-100">
        <div class="p-3">
          <h2 class="mb-1 day">{{ getDayOfWeek() }}</h2>
          <p class="text-light date mb-0">{{ date }}</p>
          <small>{{ time }}</small>
          <h2 class="place"><i class="fas fa-map-marker-alt"></i>{{ name }}<small>{{ country }}</small></h2>
          <div class="temp">
            <h1 class="weather-temp">{{ temperature }}&deg;</h1>
            <h2 class="text-light">{{ description }} <img :src="iconURL"></h2>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import DaysWeather from './DaysWeather.vue';
import moment from 'moment';

export default {
  name: 'myWeather',
  components: {
    DaysWeather
  },
  props: {
    city: String,
  },
  data() {
    return {
      cityname: this.city,
      temperature: null,
      description: null,
      iconURL: null,
      date: null,
      time: null,
      name: null,
      sea_level: null,
      wind: null,
      humidity: null,
      country: null,
      monthNames: ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December']
    };
  },
  methods: {
    changeLocation() {
      window.location.reload();
    },
    getDayOfWeek() {
      const days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];
      const date = new Date();
      return days[date.getDay()];
    },

    async fetchData() {
      try {
        const response = await axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=8331cdef4f10633c84fd856ce65588b0`);
        const { data: weatherData } = response;

        this.temperature = Math.round(weatherData.main.temp);
        this.description = weatherData.weather[0].description;
        this.name = weatherData.name;
        this.wind = weatherData.wind.speed;
        this.sea_level = weatherData.main.sea_level;
        this.country = weatherData.sys.country;
        this.humidity = weatherData.main.humidity;
        this.time = moment().format('HH:mm:ss');
        this.date = moment().format('DD-MMMM-YYYY');
        this.iconURL = `https://api.openweathermap.org/img/w/${weatherData.weather[0].icon}.png`;

        const d = new Date();
        this.date = `${d.getDate()}-${this.monthNames[d.getMonth()]}-${d.getFullYear()}`;
        this.time = `${d.getHours()}:${d.getMinutes()}:${d.getSeconds()}`;

        console.log(weatherData);
      } catch (error) {
        console.error;
      }
    }
  },
  created() {
    this.fetchData();
  }
};



</script>
  
  
<style>
body {
  background-color: rgb(201, 97, 97);
}

.weather-temp {
  margin: 0;
  font-weight: 700;
  font-size: 4em;

}


h2.day {
  font-size: 3em;
  font-weight: 400;
}

.container>.d-flex>.card.main-div {
  border-radius: 20px;

  background-image: url("https://good-nature-blog-uploads.s3.amazonaws.com/uploads/2021/10/Norway-Waterway-fin-CROP_Web.jpg");
  background-size: cover;
  background-position: center;
  background-blend-mode: overlay;

  background-repeat: no-repeat;

}

.temp {
  position: absolute;
  bottom: 0;
}

.main-div:hover {
  transform: scale(1.1);
  transition: transform 0.5s ease;
  z-index: 1;

}

.card-2 {

  border-radius: 5px;
}

.card-details {
  margin-left: 19px;
  color: blue;
}

.h1_Left {
  position: absolute;
  bottom: 25px;
  left: 16px;
  font-size: 3vw;
  line-height: 1.2;

}

.h3_Left {
  position: absolute;
  left: 16px;
  font-size: 2vw;
  line-height: 0.5;

}

.h3_Left small {
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




td {
  text-align: right;

}

table,


tr:hover {
  color: rgb(216, 224, 218);

}


.change_btn {
  background-image: linear-gradient(to right, cyan, magenta);
}

.change_btn:hover {
  transform: scale(0.9);
}


.container>.d-flex>.card {
  background-color: rgb(116, 112, 150);
  color: rgb(248, 244, 244);
}

.container>.d-flex>.card.main-div:hover {
  transform: scale(1.1);
  transition: transform 0.5s ease;
  z-index: 1;
}

.container>.d-flex>.card.main-div .temp {
  position: absolute;
  bottom: 0;
}

.container>.d-flex>.card.main-div {
  border-radius: 20px;

  background-size: cover;
  background-position: center;
  background-blend-mode: overlay;
  background-color: rgba(141, 124, 124, 0.5);
  background-repeat: no-repeat;
}

.container>.d-flex>.card.main-div:hover {
  transform: scale(1.1);
  transition: transform 0.5s ease;
  z-index: 1;
}

.container>.d-flex>.card.main-div .temp {
  position: absolute;
  bottom: 0;
}
</style>