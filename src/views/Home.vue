<template>
  <div class="home">
    <ul>
      <li v-for="city in cities" :key="city.id">{{ city.name }}</li>
    </ul>
  </div>
</template>

<script>
// import CitiesData from '@/data/city.list.min.json'
import CitiesData from '@/data/city.list.json'

export default {
  name: 'Home',
  data() {
    return {
      cities: CitiesData,
    };
  },
}
</script>
