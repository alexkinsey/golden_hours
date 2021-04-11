<template>
  <div id="app" :class="backgroundTheme == 1 ? 'sunrise' : backgroundTheme == 2 ? 'day' : backgroundTheme == 3 ? 'sunset' : 'night'">
    <h1>Golden Hour Calculator</h1>
    <location-input />
    <golden-hour-output v-if="goldenHourResult" :goldenHourResult="goldenHourResult" />
  </div>
</template>

<script>
import LocationInput from './components/LocationInput.vue';
import GoldenHourOutput from './components/GoldenHourOutput.vue';
import { eventBus } from './main.js';

export default {
  name: 'App',
  data() {
    return {
      latitude: null,
      longitude: null,
      goldenHourResult: null,
      backgroundTheme: null,
    };
  },
  components: {
    'location-input': LocationInput,
    'golden-hour-output': GoldenHourOutput,
  },
  async mounted() {
    eventBus.$on('location-data', (locationData) => {
      console.log('broadcast response');
      this.latitude = locationData.latt;
      this.longitude = locationData.longt;
      this.getSunriseSunset();
    });
    this.timeChecker();
  },
  methods: {
    async getSunriseSunset() {
      const response = await fetch(`https://api.sunrise-sunset.org/json?lat=${this.latitude}00&lng=${this.longitude}00`);
      const data = await response.json();
      this.goldenHourResult = data.results;
      const sunriseArray = this.goldenHourResult.sunrise.split(':').map((i) => Number(i));
      sunriseArray.pop();
      sunriseArray[1] = String(sunriseArray[1]).padStart(2, '0');
      const sunsetArray = this.goldenHourResult.sunset.split(':').map((i) => Number(i));
      sunsetArray.pop();
      sunsetArray[1] = String(sunsetArray[1]).padStart(2, '0');
      this.goldenHourResult.sunrise = sunriseArray;
      this.goldenHourResult.sunset = sunsetArray;
    },

    timeChecker: function() {
      var current = new Date();

      if (current.getHours() >= 5 && current.getHours() < 9) {
        this.backgroundTheme = 1;
      } else if (current.getHours() >= 9 && current.getHours() < 18) {
        this.backgroundTheme = 2;
      } else if (current.getHours() >= 18 && current.getHours() < 20) {
        this.backgroundTheme = 3;
      } else {
        this.backgroundTheme = 4
      }
    },
    // editTime: function(string) {
    //   return string.split(":")
    // }
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  height: 100vh;
  width: 100vw;
  position: absolute;
  left: 0;
  top: 0;
  color: white;
}
button:focus,
input:focus,
textarea:focus,
select:focus {
  outline: none;
}
.sunrise {
  background: rgb(223, 145, 13);
  background: linear-gradient(0deg, rgba(223, 145, 13, 1) 0%, rgba(230, 75, 0, 1) 6%, rgba(8, 8, 48, 1) 44%, rgba(11, 0, 34, 1) 100%);
  height: 100%;
}
.day {
  background: rgb(13, 181, 223);
  background: linear-gradient(0deg, rgba(13, 181, 223, 1) 0%, rgba(0, 134, 230, 1) 22%, rgba(10, 93, 230, 1) 51%, rgba(8, 0, 215, 1) 100%);
  height: 100%;
}
.sunset {
  background: rgb(255, 190, 0);
  background: linear-gradient(0deg, rgba(255, 190, 0, 1) 0%, rgba(249, 146, 0, 1) 5%, rgba(230, 79, 0, 1) 16%, rgba(16, 53, 189, 1) 53%, rgba(17, 1, 75, 1) 100%);
  height: 100%;
}
.night {
  background: rgb(0, 18, 77);
  background: linear-gradient(0deg, rgba(0, 18, 77, 1) 0%, rgba(6, 0, 27, 1) 100%);
}
</style>
