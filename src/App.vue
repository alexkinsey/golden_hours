<template>
  <div id="app" :class="backgroundTheme == 0 ? 'default' : backgroundTheme == 1 ? 'sunrise' : backgroundTheme == 2 ? 'day' : backgroundTheme == 3 ? 'sunset' : 'night'">
    <div class="main-body">
      <h1>Golden Hours</h1>
      <p class="quote">
        The Golden Hour (sometimes referred to as the Magic Hour) is often defined as the first and last hour of sunlight in the day when the special quality of light yields particularly beautiful
        photographs. The effect is particularly beautiful 25 minutes after sunrise and before sunset.
      </p>
      <location-input />
      <golden-hour-output v-if="goldenHourResult" :goldenHourResult="goldenHourResult" :locationName="locationName" />
    </div>
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
      locationName: null,
      goldenHourResult: null,
      backgroundTheme: 0,
    };
  },
  components: {
    'location-input': LocationInput,
    'golden-hour-output': GoldenHourOutput,
  },
  async mounted() {
    eventBus.$on('location-data', (locationData, city) => {
      // console.log('broadcast response');
      this.latitude = locationData.latt;
      this.longitude = locationData.longt;
      this.locationName = city;
      this.getSunriseSunset();
    });
  },
  methods: {
    async getSunriseSunset() {
      const response = await fetch(`https://api.ipgeolocation.io/astronomy?apiKey=4f2461b8883d4eb8bc5af8b2ecfcd5af&lat=${this.latitude}&long=${this.longitude}`);
      const data = await response.json();
      this.goldenHourResult = data;
      this.timeChecker(this.goldenHourResult.current_time);
    },

    timeChecker(currentTime) {
      currentTime = currentTime.split(':').map((i) => Number(i));
      // console.log(currentTime);
      if (currentTime[0] >= 5 && currentTime[0] < 9) {
        this.backgroundTheme = 1;
      } else if (currentTime[0] >= 9 && currentTime[0] < 18) {
        this.backgroundTheme = 2;
      } else if (currentTime[0] >= 18 && currentTime[0] < 20) {
        this.backgroundTheme = 3;
      } else {
        this.backgroundTheme = 4;
      }
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  height: 115%;
  width: 100%;
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
  background: rgb(255, 194, 0);
  background: linear-gradient(0deg, rgba(255, 194, 0, 1) 0%, rgba(214, 136, 4, 1) 13%, rgba(6, 55, 163, 1) 40%, rgba(6, 0, 27, 1) 100%);
  background-attachment: fixed;
}
.day {
  background: rgb(13, 181, 223);
  background: linear-gradient(0deg, rgba(13, 181, 223, 1) 0%, rgba(0, 134, 230, 1) 22%, rgba(10, 93, 230, 1) 51%, rgba(8, 0, 215, 1) 100%);
  background-attachment: fixed;
}
.sunset {
  background: rgb(255, 194, 0);
  background: linear-gradient(0deg, rgba(255, 194, 0, 1) 0%, rgba(214, 83, 4, 1) 18%, rgba(7, 47, 134, 1) 61%, rgba(6, 0, 27, 1) 100%);
  background-attachment: fixed;
}
.night {
  background: rgb(0, 18, 77);
  background: linear-gradient(0deg, rgba(0, 18, 77, 1) 0%, rgba(6, 0, 27, 1) 100%);
  background-attachment: fixed;
}
.default {
  background: black;
}

.main-body {
  width: 80%;
  max-width: 65rem;
  margin: auto;
}
.quote {
  opacity: 0.75;
  font-style: italic;
}
</style>
