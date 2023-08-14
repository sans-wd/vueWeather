<script setup>
import { ref } from "vue";

const cityCord = ref(null);
const weatherData = ref(null);
const apiKey = "fce4688c16305bbde427dda12131366d";
const search = ref("");
const showWeater = ref(false);
const weekday = [
  "Sunday",
  "Monday",
  "Tuesday",
  "Wednesday",
  "Thursday",
  "Friday",
  "Saturday",
];
const d = new Date();
const icons = {
  wind: '<font-awesome-icon icon="fa-solid fa-wind" />',
  clouds: '<font-awesome-icon icon="fa-solid fa-cloud" />',
  rain: '<font-awesome-icon icon="fa-solid fa-cloud-rain" />',
  snow: '<font-awesome-icon icon="fa-solid fa-snowflake" />',
  fog: '<font-awesome-icon icon="fa-solid fa-smog" />',
  clear: '<font-awesome-icon icon="fa-regular fa-sun" />',
};
const iconFunc = () => {
  let weaterState = cityCord.value.weather[0].main;
  weaterState = weaterState.toLowerCase();

  const weaterIcon = icons[weaterState];
  console.log(weaterIcon);
};
const fetchData = async () => {
  const url = `https://api.openweathermap.org/data/2.5/weather?q=${search.value}&appid=${apiKey}&units=metric`;
  await fetch(url)
    .then((sevindik) => sevindik.json())
    .then((data) => (cityCord.value = data));

  console.log(cityCord.value.coord);

  const url2 = `https://api.openweathermap.org/data/2.5/forecast?&units=metric&lat=${cityCord.value.coord.lat}&lon=${cityCord.value.coord.lat}&appid=${apiKey}`;
  await fetch(url2)
    .then((response) => response.json())
    .then((data) => (weatherData.value = data));

  showWeater.value = true;
  iconFunc();
};
</script>
<template>
  <main class="my-0 max-w-screen-xl mx-auto w-full h-screen">
    <div class="flex justify-center">
      <input
        v-model.trim="search"
        v-on:keyup.enter="fetchData"
        class="border-none bg-gray-400 bg-opacity-10 rounded-lg p-4 mt-7"
        type="text"
        placeholder="Search ..."
      />
    </div>

    <div class="weatherWrapper" v-if="showWeater">
      <div class="weaterBody flex justify-center items-center">
        <div
          class="weatherToday flex flex-col justify-between p-3 rounded-2xl bg-[url('https://thumbs.dreamstime.com/b/sunrise-over-beach-cancun-beautiful-mexico-40065727.jpg')] text-white w-72 h-80 bg-gray-700 bg-blend-overlay"
        >
          <div>
            <h2 class="text-3xl font-thin m-0">{{ weekday[d.getDay()] }}</h2>
            <p class="font-thin text-xl">
              {{ new Date().toLocaleDateString("de-DE") }}
            </p>
            <h1 class="font-thin text-xl mt-2">
              {{ cityCord.name }} City, {{ cityCord.sys.country }}
            </h1>
          </div>
          <div>
            <span v-html="weaterIcon"></span>
            <h2 class="text-6xl font-semibold">
              {{ Math.round(cityCord.main.temp) }}°C
            </h2>
            <h1 class="font-thin text-3xl">
              {{ cityCord.weather[0].main }}
            </h1>
          </div>
        </div>
        <div class="weatherWeek">
          <div class="hum flex justify-between items-center">
            <h1>humidity</h1>
            <p>55</p>
          </div>
          <div class="wind flex justify-between items-center">
            <h1>wind</h1>
            <p>7.2 km/h</p>
          </div>
        </div>
      </div>
      <div class="data">
        <h1>{{ cityCord }}</h1>
        <h2>{{ url2 }}</h2>
        <h1>{{ weatherData }}</h1>
      </div>
    </div>
    <div v-else>loading...</div>
  </main>
</template>

<style>
@import url("https://fonts.googleapis.com/css?family=Montserrat:400,700,900&display=swap");
main {
  font-family: "Montserrat", sans-serif;
}
</style>
