<template>
  <div class="home">
    <ul>
      <!-- <li v-for="city in cities" :key="city.id">{{ city.name }}</li> -->
      <input type="search" v-model="key" placeholder="City id" @change="searchCity(key)" />
      <p>{{ data }}</p>
    </ul>
  </div>
</template>

<script>
import axios from 'axios'
import CitiesData from '@/data/city.list.min.json'

export default {
  name: 'Home',
  data() {
    return {
      search: "",
      data: "d",
      cities: CitiesData,
    };
  },
  methods: {
    searchCity(key) {
      axios.get('https://api.openweathermap.org/data/2.5/weather?id=' + key + '&appid=2d423db4a8520eb6e988b9c97ccfebc6').then(response => {
        console.log(response)
      });
    }
  }
}
</script>
