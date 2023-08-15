<script setup>
import { ref } from "vue";

const cityCord = ref(null);
const weatherData = ref(null);
const apiKey = "fce4688c16305bbde427dda12131366d";
const search = ref("");
const showWeater = ref(false);
const weaterState = ref(null);
const weaterIcon = ref(null);
const url2 = ref(null);
const errorMsgText = ref(null);
const errorMsg = ref(false);
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
  wind: ' <i class="fa-solid fa-wind "></i>',
  clouds: '<i class="fa-solid fa-cloud "></i>',
  rain: ' <i class="fa-solid fa-cloud-rain "></i>',
  snow: '<i class="fa-solid fa-snowflake"></i>',
  mist: ' <i class="fa-solid fa-smog"></i>',
  clear: '<i class="fa-solid fa-sun "></i> ',
};
const fetchData = async () => {
  const url = `https://api.openweathermap.org/data/2.5/weather?q=${search.value}&appid=${apiKey}&units=metric`;
  try {
    errorMsg.value = false;
    await fetch(url)
      .then((sevindik) => sevindik.json())
      .then((data) => (cityCord.value = data))
      .then(() => {
        url2.value = `https://api.openweathermap.org/data/2.5/forecast?&units=metric&lat=${cityCord.value.coord.lat}&lon=${cityCord.value.coord.lon}&cnt=5&appid=${apiKey}`;
        fetch(url2.value)
          .then((response) => response.json())
          .then((data) => (weatherData.value = data))
          .then(() => {
            weaterState.value = cityCord.value.weather[0].main;
            weaterState.value = weaterState.value.toLowerCase();

            weaterIcon.value = icons[weaterState.value];
            console.log(weaterIcon);
            showWeater.value = true;
          });
      });
  } catch (error) {
    console.log(error);
    errorMsgText.value = "Incorrect city, try again";
    errorMsg.value = true;
  }
};
</script>
<template>
  <main
    class="my-0 max-w-screen-xl mx-auto w-full h-screen flex flex-col justify-center items-center"
  >
    <Transition name="bounce">
      <div v-if="!showWeater" class="flex justify-center">
        <input
          v-model.trim="search"
          v-on:keyup.enter="fetchData"
          class="border-none bg-gray-400 bg-opacity-10 rounded-lg p-4 mt-7"
          type="text"
          placeholder="Search ..."
        />
      </div>
    </Transition>
    <Transition name="bounce">
      <div class="weatherWrapper" v-if="showWeater">
        <div class="weaterBody flex justify-center items-center">
          <div
            class="weatherToday flex flex-col justify-between p-3 rounded-2xl bg-[url('https://thumbs.dreamstime.com/b/sunrise-over-beach-cancun-beautiful-mexico-40065727.jpg')] text-white w-96 h-80 bg-gray-700 bg-blend-overlay transition-all hover:scale-110 scale-105 shadow-md z-10"
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
              <span class="text-3xl" v-html="weaterIcon"></span>

              <h2 class="text-6xl font-semibold">
                {{ Math.round(cityCord.main.temp) }}°C
              </h2>
              <h1 class="font-thin text-3xl">
                {{ cityCord.weather[0].main }}
              </h1>
            </div>
          </div>
          <div
            class="weatherWeek bg-gray-950 text-white max-w-md w-full h-80 p-5 rounded-tr-2xl rounded-br-2xl flex flex-col justify-between"
          >
            <div>
              <div class="hum flex justify-between items-center">
                <h1 class="uppercase">humidity</h1>
                <p>{{ cityCord.main.humidity }}%</p>
              </div>
              <div class="wind flex justify-between items-center">
                <h1 class="uppercase">wind</h1>
                <p>{{ cityCord.wind.speed }} km/h</p>
              </div>
            </div>
            <div
              class="weekWrapper flex ml-0 gap-2 justify-center items-center text-white"
            >
              <div class="w-1/5">
                <div
                  class="w-full hover:scale-105 transition shadow-md shadow-teal-500 h-28 rounded-xl p-1 bg-teal-950 flex justify-between items-center flex-col"
                >
                  <p>12:00</p>
                  <p
                    class="text-3xl"
                    v-html="
                      icons[weatherData.list[0].weather[0].main.toLowerCase()]
                    "
                  ></p>
                  <p>{{ Math.round(weatherData.list[0].main.temp) }}°C</p>
                </div>
              </div>
              <div class="w-1/5">
                <div
                  class="w-full hover:scale-105 transition shadow-md shadow-teal-500 h-28 rounded-xl p-1 bg-teal-950 flex justify-between items-center flex-col"
                >
                  <p>15:00</p>
                  <p
                    class="text-3xl"
                    v-html="
                      icons[weatherData.list[1].weather[0].main.toLowerCase()]
                    "
                  ></p>
                  <p>{{ Math.round(weatherData.list[1].main.temp) }}°C</p>
                </div>
              </div>
              <div class="w-1/5">
                <div
                  class="w-full hover:scale-105 transition shadow-md shadow-teal-500 h-28 rounded-xl p-1 bg-teal-950 flex justify-between items-center flex-col"
                >
                  <p>18:00</p>
                  <p
                    class="text-3xl"
                    v-html="
                      icons[weatherData.list[2].weather[0].main.toLowerCase()]
                    "
                  ></p>
                  <p>{{ Math.round(weatherData.list[2].main.temp) }}°C</p>
                </div>
              </div>
              <div class="w-1/5">
                <div
                  class="w-full hover:scale-105 transition shadow-md shadow-teal-500 h-28 rounded-xl p-1 bg-teal-950 flex justify-between items-center flex-col"
                >
                  <p>21:00</p>
                  <p
                    class="text-3xl"
                    v-html="
                      icons[weatherData.list[3].weather[0].main.toLowerCase()]
                    "
                  ></p>
                  <p>{{ Math.round(weatherData.list[3].main.temp) }}°C</p>
                </div>
              </div>
              <div class="w-1/5">
                <div
                  class="w-full hover:scale-105 transition shadow-md shadow-teal-500 h-28 rounded-xl p-1 bg-teal-950 flex justify-between items-center flex-col"
                >
                  <p>00:00</p>
                  <p
                    class="text-3xl"
                    v-html="
                      icons[weatherData.list[4].weather[0].main.toLowerCase()]
                    "
                  ></p>
                  <p>{{ Math.round(weatherData.list[4].main.temp) }}°C</p>
                </div>
              </div>
            </div>
            <button
              @click="
                showWeater = false;
                search = '';
              "
              class="bg-gradient-to-r from-indigo-500 via-purple-500 to-pink-500 w-full p-2 rounded-full"
            >
              Change location
            </button>
          </div>
        </div>
        <!-- <div class="data">
          <pre>
            <h1>{{ cityCord }}</h1>
          </pre>
          <h2>{{ url2 }}</h2>
          <pre>
          <h1>{{ weatherData }}</h1>
        </pre>
        </div> -->
      </div>
    </Transition>
    <Transition name="bounce">
      <div v-if="errorMsg">
        {{ errorMsgText }}
      </div>
    </Transition>
  </main>
</template>

<style>
@import url("https://fonts.googleapis.com/css?family=Montserrat:400,700,900&display=swap");
main {
  font-family: "Montserrat", sans-serif;
}
.bounce-enter-active {
  animation: bounce-in 0.3s;
}
.bounce-leave-active {
  animation: bounce-in 0.3s reverse;
}
@keyframes bounce-in {
  0% {
    transform: scale(0);
  }
  50% {
    transform: scale(1.25);
  }
  100% {
    transform: scale(1);
  }
}
</style>
