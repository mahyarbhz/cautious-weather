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
        <div class="container weather-container" v-if="fetchedCities">
            <div class="weather-card" v-for="(city, index) in fetchedCities" :key="index">
                <div class="weather-card-header">
                    <h3>{{ city.name }}</h3>
                </div>
                <div class="weather-card-body">
                    <h4>
                        <img alt="sunny-outline" src="@/assets/svgs/sunny-outline.svg" v-if="city.weather[0].main == 'Clear'">
                        <img alt="cloudy-outline" src="@/assets/svgs/cloudy-outline.svg" v-else-if="city.weather[0].main == 'Clouds'">
                        <img alt="rainy-outline" src="@/assets/svgs/rainy-outline.svg" v-else-if="city.weather[0].main == 'x'">
                        <img alt="thunderstorm-outline" src="@/assets/svgs/thunderstorm-outline.svg" v-else-if="city.weather[0].main == 'y'">
                        <img alt="cloud-circle-outline" src="@/assets/svgs/cloud-circle-outline.svg" v-else>
                        {{ city.weather[0].main }}</h4>
                    <h4><img alt="thermometer-outline" src="@/assets/svgs/thermometer-outline.svg"> {{ city.main.temp }}</h4>
                </div>
            </div>
        </div>
        <div class="container weather-container weather-container-history" v-if="historyFetched">
            <div class="weather-card weather-card-history bg-red" v-for="(city, index) in historyFetched" :key="index">
                <div class="weather-card-header">
                    <h3>{{ city.name }}</h3>
                </div>
                <div class="weather-card-body">
                    <h4>
                        <img alt="sunny-outline" src="@/assets/svgs/sunny-outline.svg" v-if="city.weather[0].main == 'Clear'">
                        <img alt="cloudy-outline" src="@/assets/svgs/cloudy-outline.svg" v-else-if="city.weather[0].main == 'Clouds'">
                        <img alt="rainy-outline" src="@/assets/svgs/rainy-outline.svg" v-else-if="city.weather[0].main == 'x'">
                        <img alt="thunderstorm-outline" src="@/assets/svgs/thunderstorm-outline.svg" v-else-if="city.weather[0].main == 'y'">
                        <img alt="cloud-circle-outline" src="@/assets/svgs/cloud-circle-outline.svg" v-else>
                        {{ city.weather[0].main }}</h4>
                    <h4><img alt="thermometer-outline" src="@/assets/svgs/thermometer-outline.svg"> {{ city.main.temp }}</h4>
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
                defaultCities: ['Tehran', 'Los Angeles', 'Paris', 'Tokyo', 'Shiraz'],
                fetchedCities: [],
                history: [],
                historyFetched: [],
            };
        },
        mounted() {
            this.history = JSON.parse(localStorage.getItem("history")) || []
            this.$nextTick(function () {
                this.defaultCities.forEach((element) => {
                    axios.get('https://api.openweathermap.org/data/2.5/weather?q=' + element + '&appid=2d423db4a8520eb6e988b9c97ccfebc6&units=metric')
                    .then(response => {
                        this.fetchedCities.push(response.data)
                    })
                    .catch(error => {
                        console.log(error)
                    })
                })
                this.history.forEach((element) => {
                    axios.get('https://api.openweathermap.org/data/2.5/weather?q=' + element + '&appid=2d423db4a8520eb6e988b9c97ccfebc6&units=metric')
                    .then(response => {
                        this.historyFetched.push(response.data)
                    })
                    .catch(error => {
                        console.log(error)
                    })
                })
            })
        },
        methods: {
            searchCity() {
                axios.get('https://api.openweathermap.org/data/2.5/weather?q=' + this.cityName + '&appid=2d423db4a8520eb6e988b9c97ccfebc6&units=metric')
                .then(response => {
                    this.loadedCity = response.data
                    let cityName = response.data.name.toLowerCase()
                    if (!this.history.includes(cityName)) {
                        this.history.push(cityName)
                        localStorage.setItem("history", JSON.stringify(this.history))
                        this.historyFetched.push(response.data)
                    }
                })
                .catch(error => {
                    alert(error.response.data.message);
                });
            },
            
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

    .bg-red {
        background-color: rgb(212, 54, 54) !important;
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

            &-history {
                justify-content: center;
            }
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

            &-history {
                margin: 2rem 2rem 0;
            }

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

                    &>img {
                        filter: invert(100%);
                        height: 20px;
                    }
                }
            }
        }
    }
</style>