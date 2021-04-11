<template>
  <form @submit.prevent="submitLocation()">
    <label for="location-input">Enter a location</label>
    <input type="text" name="location-input" id="location-input" placeholder="City" v-model="city" />
    <input type="text" name="location-input" id="location-input" placeholder="Country" v-model="country" />
    <input type="submit" value="Submit" class="submit-button"/>
  </form>
</template>

<script>
import { eventBus } from '../main.js';

export default {
  name: 'LocationInput',
  data() {
    return {
      city: '',
      country: '',
      locationData: null,
    };
  },
  methods: {
    async submitLocation() {
      console.log('click');
      const response = await fetch(`https://geocode.xyz/${this.city},${this.country}?json=1`)
      const data = await response.json()
      this.locationData =  data
      this.broadcastData();
    },
    broadcastData: function() {
      eventBus.$emit('location-data', this.locationData);
    },
  },
};
</script>

<style scoped>
label {
  margin: 0.5rem;
}
.submit-button {
  margin: 0.5rem;
  padding: 0.3rem 0.5rem 0.3rem 0.5rem;
  background-color: rgb(16, 84, 173);
  color: white;
  border: none;
  border-radius: 0.2rem;
}
.submit-button:hover {
  background-color: rgb(59, 145, 224);
}
.submit-button:active {
  background-color: rgb(14, 71, 124);
}
</style>
