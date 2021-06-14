<template>
  <div>
    <div class="info-box-container">
      <h2>
        Time in <span class="location-capitalize">{{ locationName }}</span> {{ goldenHourResult.current_time.slice(0, 5) }}
      </h2>
      <div class="info-box">
        <span>Morning Blue Hour</span><span class="accent-blue">{{ timeOffSet(goldenHourResult.sunrise, -45) }}</span>
      </div>
      <div class="info-box">
        <span>Sunrise</span><span class="accent-orange">{{ goldenHourResult.sunrise }}</span>
      </div>
      <div class="info-box">
        <span>Morning Golden Hour</span><span class="accent-gold">{{ timeOffSet(goldenHourResult.sunrise, 25) }}</span>
      </div>
      <div class="info-box">
        <span>Solar Noon</span><span class="accent-lightblue">{{ goldenHourResult.solar_noon }}</span>
      </div>
      <div class="info-box">
        <span>Evening Golden Hour</span><span class="accent-gold">{{ timeOffSet(goldenHourResult.sunset, -25) }}</span>
      </div>
      <div class="info-box">
        <span>Sunset</span><span class="accent-orange">{{ goldenHourResult.sunset }}</span>
      </div>
      <div class="info-box">
        <span>Evening Blue Hour</span><span class="accent-blue">{{ timeOffSet(goldenHourResult.sunset, 45) }}</span>
      </div>
      <div class="info-box">
        <span>Moonrise</span><span class="accent-grey">{{ goldenHourResult.moonrise }}</span>
      </div>
      <div class="info-box">
        <span>Moonset</span><span class="accent-grey">{{ goldenHourResult.moonset }}</span>
      </div>
      <div class="info-box">
        <span>Daylight</span><span class="accent-lightblue">{{ goldenHourResult.day_length.slice(0, 2) }} hr, {{ goldenHourResult.day_length.slice(3, 5) }} min</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'GoldenHourOutput',
  props: ['goldenHourResult', 'locationName'],
  data() {
    return {};
  },
  methods: {
    timeOffSet: function(time, offSet) {
      // console.log(time, offSet);
      const timeArray = time.split(':').map((i) => Number(i));
      timeArray[1] += offSet;
      if (timeArray[1] > 59) {
        timeArray[1] -= 60;
        timeArray[0] += 1;
        if (timeArray[0] > 23) {
          timeArray[0] = 0;
        }
      } else if (timeArray[1] < 0) {
        timeArray[1] += 60;
        timeArray[0] -= 1;
        if (timeArray[0] < 0) {
          timeArray[0] = 23;
        }
      }
      return String(timeArray[0]).padStart(2, '0') + ':' + String(timeArray[1]).padStart(2, '0');
    },
  },
};
</script>

<style scoped>
.location-capitalize {
  text-transform: capitalize;
}
.info-box-container {
  /* background: rgba(0, 0, 0, 0.199); */
  /* padding: 1rem 0 2rem 0; */
  margin-top: 1rem;
  min-width: 17rem;
}
.info-box {
  border-radius: 1rem;
  background-color: rgba(0, 0, 0, 0.7);
  /* width: 15rem; */
  /* width: 90%; */
  min-width: 15rem;
  max-width: 25rem;
  margin: 1rem auto;
  display: flex;
  justify-content: space-between;
  padding: 1rem;
  transition: .25s;
}
.info-box:hover {
  background-color: rgba(19, 19, 51, 0.877);
  box-shadow: 0 0 10px 5px rgba(23, 23, 173, 0.699);
}
.accent-blue {
  color: rgb(44, 135, 255);
  font-weight: bolder;
}
.accent-lightblue {
  color: rgb(97, 237, 255);
  font-weight: 600;
}
.accent-gold {
  color: gold;
  font-weight: 600;
}
.accent-orange {
  color: orange;
  font-weight: 600;
}
.accent-grey {
  color: rgb(207, 176, 168);
  font-weight: 600;
}
</style>
