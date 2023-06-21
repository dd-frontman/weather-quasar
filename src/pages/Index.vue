<template lang='pug'>
q-page.flex.column(:class="[weatherData?.weather[0].icon.endsWith('n') ? 'bg-night' : 'bg-day']")
  .col.q-py-lg.q-px-lg
    q-input(v-model='search' @keyup.enter='getWeatherBySearch' label='Search your city' dark)
      template(v-slot:prepend)
        q-icon(name='place')
      q-btn.cursor-pointer(round dense flat icon='search' @click='getWeatherBySearch')
  .col.text-white.text-center
    .col.text-h2.text-weight-thin
      | Quasar
      br
      | Weather
    q-btn.col(@click='getLocation' size='1.5em' flat='' icon='my_location')
      .q-mx-md Find my location
  template(v-if='weatherData')
    .col.text-white.text-center
      .text-h4.text-weight-light
        | {{ weatherData.name }}
      .text-h6.text-weight-light
        | {{ weatherData.weather[0].description }}
      .text-h1.text.weight-thin.q-my-lg.relative-position
        span {{ Math.round(weatherData.main.temp) }}
        span.text-h4.relative-position.degree °C
      img.img-weather.bg-white(:src='`http://openweathermap.org/img/wn/${weatherData.weather[0].icon}@2x.png`' alt='weather')
  .col.city
</template>

<script setup lang='ts'>
import { ref } from 'vue';
import axios from 'axios';

interface WeatherData {
  name: string
  weather: {
    description: string
    icon: string
  }[]
  main: {
    temp: number
  }
}

const search = ref('')
const weatherData = ref<WeatherData | null>(null)
const URL = 'https://api.openweathermap.org/data/2.5/weather?'
const KEY = 'e97367c70002d8289087dc404664352d'

async function getWeather (url: string) {
  try {
    const { data } = await axios.get<WeatherData>(url);
    weatherData.value = data;
  } catch(e) {
    console.log(e)
    alert('Nothing found, enter again.')
  }
}

async function getLocation () {
  if (!navigator.geolocation) return;

  const position = await new Promise<GeolocationPosition>((resolve, reject) =>
    navigator.geolocation.getCurrentPosition(resolve, reject)
  );

  const { coords: { latitude, longitude } } = position;
  const url = `${URL}lat=${latitude}&lon=${longitude}&appid=${KEY}&units=metric`;
  await getWeather(url);
}

async function getWeatherBySearch () {
  const url = `${URL}q=${search.value}&appid=${KEY}&units=metric`
  await getWeather(url)
}
</script>

<style lang='sass'>

$yellow: #fdbb2d
$red: #f12711
$blue: #1a2a6c
$gray: #373B44

.q-page
  background: linear-gradient(to right, $red, $yellow, $blue)

.bg-night
  background: linear-gradient(to right, $blue, $gray)

.bg-day
  background: linear-gradient(to right, $yellow, $red)

.degree
  top: -44px

.img-weather
  border-radius: 3rem

.city
  background: url('../../public/img/city.png') bottom center
  flex: 0 0 200px
  background-size: contain
  @media (max-width: 445px)
    background-size: cover
</style>
