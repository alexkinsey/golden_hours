<template>
  <form @submit.prevent="submitLocation()">
    <!-- <label for="location-input">Enter a location</label> -->
    <input type="text" name="location-city-input" id="location-input" placeholder="Enter name of city" required class="input-field" v-model="city" />
    <input type="text" name="location-country-input" id="location-input" placeholder="Enter name of country" required class="input-field" v-model="country" />
    <input type="submit" value="Submit" class="search-button" />
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
      // console.log('click');
      const response = await fetch(`https://geocode.xyz/${this.city},${this.country}?json=1`);
      const data = await response.json();
      this.locationData = data;
      this.broadcastData();
    },
    broadcastData: function() {
      eventBus.$emit('location-data', this.locationData, this.city);
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

form {
  display: flex;
  margin: auto;
  min-width: 15rem;
  max-width: 27rem;
  flex-direction: column;
}

.input-field {
  padding: 0.75rem;
  background-color: transparent;
  color: white;
  border-radius: 8px;
  border: 1px solid #ffffff;
  margin: 0.2rem 0;
  font-size: 16px;
}
.input-field:hover {
  background-color: rgba(255, 255, 255, 0.075);
}

.search-button {
  background-color: transparent;
  border-radius: 8px;
  border: 1px solid #ffffff;
  display: inline-block;
  cursor: pointer;
  color: #ffffff;
  font-size: 16px;
  padding: 10px 20px;
  text-decoration: none;
  margin: 0.2rem 0;
}
@media (hover: hover) {
  .search-button:hover {
    background-color: rgba(255, 255, 255, 0.075);
    padding: 10px 20px;
  }
}

.search-button:active {
  position: relative;
  padding: 10px 20px;
  top: 1px;
}
</style>
