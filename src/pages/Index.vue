<template lang='pug'>
q-page.flex.column(:class='day')
  .col.q-py-lg.q-px-lg
    q-input(v-model='search' @keyup.enter='getWeatherBySearch' label='Search your city' dark)
      template(v-slot:prepend='')
        q-icon(name='place')
      q-btn.cursor-pointer(round='' dense='' flat='' icon='search' @click='getWeatherBySearch')
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

const search = ref('');
const weatherData = ref(null);
const lat = ref(0);
const long = ref(0);
const apiUrl = 'https://api.openweathermap.org/data/2.5/weather?';
const apiKey = 'e97367c70002d8289087dc404664352d';
const day = ref('');

const getLocation = (() => {
  return navigator.geolocation.getCurrentPosition(position => {
    lat.value = position.coords.latitude;
    long.value = position.coords.longitude;
    getWeatherByCoords();
  });
});
const getWeatherByCoords = (() => {
  void axios(`${apiUrl}lat=${lat.value}&lon=${long.value}&appid=${apiKey}&units=metric`)
    .then((response) => {
      // eslint-disable-next-line @typescript-eslint/no-unsafe-assignment,@typescript-eslint/no-unsafe-return
      weatherData.value = response.data;
      // eslint-disable-next-line @typescript-eslint/no-unsafe-call,@typescript-eslint/no-unsafe-member-access
      day.value = response.data.weather[0].icon.endsWith('n') ? 'bg-night' : 'bg-day';
    }).catch(() => alert('Nothing found, try again later'));

});
const getWeatherBySearch = (() => {
  void axios(`${apiUrl}q=${search.value}&appid=${apiKey}&units=metric`)
    .then((response) => {
      // eslint-disable-next-line @typescript-eslint/no-unsafe-assignment,@typescript-eslint/no-unsafe-return
      weatherData.value = response.data;
      // eslint-disable-next-line @typescript-eslint/no-unsafe-call,@typescript-eslint/no-unsafe-member-access
      day.value = response.data.weather[0].icon.endsWith('n') ? 'bg-night' : 'bg-day';
    }).catch(() => alert('Nothing found, enter again.'));
});
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
