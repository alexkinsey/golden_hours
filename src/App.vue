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
      <h3 v-if="errorCode">{{ errorCode }}</h3>
      <h3 v-if="errorCode">Try again!</h3>
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
      errorCode: 0,
    };
  },
  components: {
    'location-input': LocationInput,
    'golden-hour-output': GoldenHourOutput,
  },
  async mounted() {
    eventBus.$on('location-data', (locationData, city) => {
      // console.log('broadcast response');
      if (locationData['error']) {
        this.goldenHourResult = null;
        if (locationData.error.description) {
          this.errorCode = locationData.error.description;
        } else {
          this.errorCode = locationData.error.message;
        }
      } else {
        this.errorCode = 0;
        this.latitude = locationData.latt;
        this.longitude = locationData.longt;
        this.locationName = city;
        this.getSunriseSunset();
      }
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
  /* background-attachment: fixed; */
  height: 100%;
  width: 100%;
  /* position: absolute; */
  position: fixed;
  display: flex;
  flex-direction: column;
  overflow: hidden;
  left: 0;
  top: 0;
  color: white;
  font-size: 16px;
  background-color: black;
}

button:focus,
input:focus,
textarea:focus,
select:focus {
  outline: none;
}

input {
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
}

.sunrise {
  background: rgb(3, 7, 34);
  background: linear-gradient(180deg, rgba(3, 7, 34, 1) 0%, rgba(6, 73, 144, 1) 40%, rgba(53, 116, 167, 1) 60%, rgba(253, 57, 29, 1) 83%, rgb(252, 188, 69) 100%);
  background-attachment: fixed;
}
.day {
  background: rgb(13, 181, 223);
  background: linear-gradient(0deg, rgba(13, 181, 223, 1) 0%, rgba(0, 134, 230, 1) 22%, rgba(10, 93, 230, 1) 51%, rgba(8, 0, 215, 1) 100%);
  background-attachment: fixed;
}
.sunset {
  background: rgb(17,16,54);
background: linear-gradient(180deg, rgba(17,16,54,1) 0%, rgba(22,75,130,1) 38%, rgba(136,58,74,1) 52%, rgba(170,69,33,1) 64%, rgba(226,94,21,1) 83%, rgba(221,161,16,1) 100%);
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
  width: clamp(90%, 80%, 65rem);
  max-width: 65rem;
  margin: 0 auto auto auto;
  padding: 1rem;
  /* padding: env(safe-area-inset-bottom); */
  overflow-y: auto;
  overflow-x: hidden;
  transition: 0.25s;
}
.quote {
  opacity: 0.75;
  font-style: italic;
}
</style>
