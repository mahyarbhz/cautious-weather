<template>
    <div class="home">
        <ul>
            <form class="weather-form" @submit.prevent="searchCity">
                <input type="search" v-model="cityName" placeholder="City name" />
                <input type="submit" value="Search">
            </form>
        </ul>
        <div class="weather-container">
            <div class="weather-card" v-if="loadedCity">
                <h3>{{ loadedCity.name }}</h3>
                <h4>{{ loadedCity.main.temp }}</h4>
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
            };
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
        &-container {
            display: flex;
            justify-content: center;
        }
        &-card {
            width: 200px;
            height: 300px;
            background-color: rgba(20, 128, 200, 1);
            color: rgba(255, 255, 255, 1);
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
            transition: all 0.25s ease;
        }
    }
</style>