<template>
  <div>
    <p>Note all times are shown at UTC. Summertime adjustments are not included.</p>
    <!-- <p>Sunrise: Golden hour will begin at {{ goldenHourResult.sunrise }} AM</p> -->
    <!-- <p>Sunset: Golden hour will begin at {{ goldenHourResult.sunset }} PM</p> -->
    <p>Sunrise: Golden hour will begin at {{ goldenHourResult.sunset[0] }}:{{ goldenHourResult.sunset[1] }} AM and end at {{ goldenHourResult.sunset[0]+1 }}:{{ goldenHourResult.sunset[1] }} AM</p>
    <p>Sunset: Golden hour will begin at {{ goldenHourResult.sunset[0]-1 }}:{{ goldenHourResult.sunset[1] }} PM and end at {{ goldenHourResult.sunset[0] }}:{{ goldenHourResult.sunset[1] }} PM </p>
  </div>
</template>

<script>
export default {
  name: 'GoldenHourOutput',
  props: ['goldenHourResult'],
  data() {
    return {
      numericSunset: null,
    };
  },
  // mounted() {
  //   this.timeOffSet()
  // },
  // methods: {
  //    timeOffSet(){
  //     this.numericSunset =  this.goldenHourResult.sunset.split(":");
  //   }
  // },
};
</script>

<style></style>
