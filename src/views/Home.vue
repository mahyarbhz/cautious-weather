<template>
    <div class="home">
        <ul>
            <form class="weather-form" @submit.prevent="searchCity">
                <input type="search" v-model="cityName" placeholder="City name" />
                <input type="submit" value="Search">
            </form>
        </ul>
        <div class="container" v-if="loadedCity">
            <div class="weather-result">
                <h3>{{ loadedCity.name }}</h3>
                <h4>{{ loadedCity.main.temp }}</h4>
            </div>
        </div>
        <div class="container weather-container" v-if="loadedCities">
            <div class="weather-card" v-for="(city, index) in loadedCities" :key="index">
                <div class="weather-card-header">
                    <h3>{{ city.name }}</h3>
                </div>
                <div class="weather-card-body">
                    <h4>
                        <ion-icon name="sunny-outline" v-if="city.weather[0].main == 'Clear'"></ion-icon>
                        <ion-icon name="cloudy-outline" v-else-if="city.weather[0].main == 'Clouds'"></ion-icon>
                        <ion-icon name="rainy-outline" v-else-if="city.weather[0].main == 'x'"></ion-icon>
                        <ion-icon name="thunderstorm-outline" v-else-if="city.weather[0].main == 'y'"></ion-icon>
                        <ion-icon name="cloud-circle-outline" v-else></ion-icon>
                        {{ city.weather[0].main }}</h4>
                    <h4><ion-icon name="thermometer-outline"></ion-icon> {{ city.main.temp }}</h4>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import axios from 'axios'

    export default {
        name: 'Home',
        data() {
            return {
                cityName: "",
                loadedCity: null,
                loadedCities: []
            };
        },
        mounted() {
            this.$nextTick(function () {
                axios.get('https://api.openweathermap.org/data/2.5/weather?q=' + 'Tehran' + '&appid=2d423db4a8520eb6e988b9c97ccfebc6&units=metric')
                .then(response => {
                    this.loadedCities.push(response.data);
                })
                .catch(error => {
                    console.log(error);
                });

                axios.get('https://api.openweathermap.org/data/2.5/weather?q=' + 'Los Angeles' + '&appid=2d423db4a8520eb6e988b9c97ccfebc6&units=metric')
                .then(response => {
                    this.loadedCities.push(response.data);
                })
                .catch(error => {
                    console.log(error);
                });

                axios.get('https://api.openweathermap.org/data/2.5/weather?q=' + 'Paris' + '&appid=2d423db4a8520eb6e988b9c97ccfebc6&units=metric')
                .then(response => {
                    this.loadedCities.push(response.data);
                })
                .catch(error => {
                    console.log(error);
                });

                axios.get('https://api.openweathermap.org/data/2.5/weather?q=' + 'Tokyo' + '&appid=2d423db4a8520eb6e988b9c97ccfebc6&units=metric')
                .then(response => {
                    this.loadedCities.push(response.data);
                })
                .catch(error => {
                    console.log(error);
                });

                axios.get('https://api.openweathermap.org/data/2.5/weather?q=' + 'Shiraz' + '&appid=2d423db4a8520eb6e988b9c97ccfebc6&units=metric')
                .then(response => {
                    this.loadedCities.push(response.data);
                })
                .catch(error => {
                    console.log(error);
                });

                console.log(this.loadedCities);
            });
        },
        methods: {
            searchCity() {
                axios.get('https://api.openweathermap.org/data/2.5/weather?q=' + this.cityName + '&appid=2d423db4a8520eb6e988b9c97ccfebc6&units=metric')
                .then(response => {
                    this.loadedCity = response.data;
                })
                .catch(error => {
                    alert(error.response.data.message);
                });
            }
        }
    }
</script>

<style lang="scss" scoped>
    ul {
        padding: 0;
    }

    .container {
        width: 1360px;
        margin: 2rem auto;
    }

    .weather {
        &-form {
            input[type="search"] {
                padding: 6px 12px;
                margin: 0 5px;
                border: 2px solid rgba(13, 16, 212, 0);
                border-radius: 5px;
                box-shadow: 0 0 12px rgba(0, 0, 0, 0.5);
                outline: none;
                transition: all .5s ease;
                font-size: 14px;

                &:focus {
                    border: 2px solid rgba(13, 16, 212, 1);
                }
            }

            input[type="submit"] {
                padding: 6px 12px;
                border: 2px solid rgba(0, 0, 0, 0);
                border-radius: 5px;
                font-size: 14px;
                box-shadow: 0 0 12px rgba(0, 0, 0, 0.5);
                transition: all .5s ease;
            }
        }
        &-result {
            background-color: rgb(212, 54, 54);
            box-shadow: 0 0 5px rgba(0, 0, 0, 0.4);
            color: rgba(255, 255, 255, 1);
            border-radius: 8px;
            padding: 12px;
        }
        &-container {
            display: flex;
            justify-content: space-between;
            flex-wrap: wrap;
        }
        &-card {
            width: 200px;
            height: 300px;
            margin: 8px 0;
            background-color: rgba(20, 128, 200, 1);
            color: rgba(255, 255, 255, 1);
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
            transition: all 0.25s ease;

            &-header {
                text-decoration: underline;
            }

            &-body {
                text-align: start;
                padding: 0 12px;

                &>* {
                    display: flex;
                    align-items: center;

                    &>* {
                        margin: 0 4px;
                    }

                    &>ion-icon {
                        font-size: 20px;
                    }
                }
            }
        }
    }
</style>