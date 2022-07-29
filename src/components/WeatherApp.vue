<template>
  <div
    id="bg-cold"
    :class="[
      typeof weather.main !== 'undefined' && weather.main.temp > 24
        ? 'rain'
        : '',
      'bg-cover bg-bottom bg-no-repeat',
    ]"
  >
    <main
      class="min-h-screen bg-gradient-to-b p-[25px] from-black/25 to-black/75"
    >
      <div class="container mx-auto">
        <div id="search" class="w-full mb-4">
          <input
            type="text"
            placeholder="search..."
            class="rounded-tl-2xl rounded-br-2xl focus:outline-none focus:rounded-bl-2xl focus:rounded-tr-2xl focus:rounded-tl-none focus:rounded-br-none w-full p-2 bg-[rgba(255,255,255,.5)] focus:bg-[rgba(255,255,255,.7)] ease-in-out duration-300 shadow-[0px_0px_8px_rgba(0,0,0,.4)]"
            v-model="weatherSearch"
            @keypress="fetchWeather"
          />
        </div>

        <div v-if="!error">
          <div id="wrapper" v-if="typeof weather.main !== 'undefined'">
            <div class="text-center space-y-3">
              <div
                class="text-[40px] font-semibold text-white capitalize drop-shadow-[1px_4px_rgba(0,0,0,.25)]"
              >
                {{ weather.name }}, {{ weather.sys.country }}
              </div>
              <div class="text-sm italic text-white font-medium">{{dateBuilder()}}</div>
            </div>
            <div
              class="bg-[rgba(255,255,255,.55)] w-10/12 mx-auto mt-6 rounded-2xl shadow-[3px_6px_rgba(0,0,0,0.25)] text-center"
            >
              <div
                class="text-[102px] text-white inline-block drop-shadow-[1px_3px_rgba(0,0,0,25)] px-[20px]"
              >
                {{ Math.round(weather.main.temp) }}°c
              </div>
            </div>
            <div
              id="weather_condition"
              class="italic text-[40px] text-center font-semibold drop-shadow-[3px_6px_rgba(0,0,0,0.25)] text-white capitalize"
            >
              {{ weather.weather[0].main }}
            </div>
          </div>
        </div>
        <div
          v-else
          class="bg-[rgba(255,255,255,.55)] w-10/12 mx-auto mt-6 rounded-2xl shadow-[3px_6px_rgba(0,0,0,0.25)] text-center"
        >
          <div
            class="text-[48px] text-white inline-block drop-shadow-[1px_3px_rgba(0,0,0,25)] px-[20px]"
          >
            {{ error }}
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script lang="ts">
import { defineComponent, onMounted, ref } from "vue";
import axios from "axios";
import env from "@/env.js";

export default defineComponent({
  setup() {
    const weatherSearch = ref<string>("");
    const weather = ref<[]>([]);
    //const loader = ref<null>(null);
    const error = ref<string>("");

    async function fetchWeather(e: any) {
      if (e.key === "Enter") {
        try {
          await axios(
            `${env.base_url}weather?q=${weatherSearch.value}&units=metric&APPID=${env.api_key}`
          )
            .then((res: any) => res.data)
            .then(setResults);
          //.then((data) => (loader.value = data))
        } catch (er: any) {
          error.value = er.response.data.message;
          weatherSearch.value = "";
        }
      }
    }
    function setResults(result: []) {
      weather.value = result;
      weatherSearch.value = "";
    }

    function dateBuilder() {
      const d = new Date();
      const months: string[] = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      const days: string[] = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    }
    return { fetchWeather, weatherSearch, weather, /* loader,*/ error, dateBuilder };
  },

  methods: {},
});
</script>

<style scoped>
#bg-cold {
  background-image: url("../assets/image/cold-bg.jpg");
  height: 100vh;
}
#bg-cold.rain {
  background-image: url("../assets/image/rain-bg.gif");
}
</style>
