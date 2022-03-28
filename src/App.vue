<template>
  <div>
    <div class="layout flex min-h-screen items-center justify-center bg-teal-500">
      <div class="wrapper bg-white shadow-md rounded-md w-96">
        <header
          class="flex text-xl font-bold p-4 text-teal-500 items-center border-b border-b-slate-400"
        >
          <box-icon
            v-on:click="returnForm"
            v-if="isActive"
            name="left-arrow-alt"
            class="text-lg cursor-pointer mr-2 text-teal-500"
          ></box-icon>Погода
        </header>
        <div v-if="isActive == false">
          <section class="input-part m-8">
            <p class="info-txt hidden text-center rounded-md text-md mb-4 py-4 px-2"></p>
            <div class="content">
              <input
                v-model="query"
                @keypress="fetchWeather"
                type="text"
                class="w-full h-14 rounded-md text-md text-center px-4 border border-slate-300 placeholder:text-slate-400 focus:border-2 focus:border-teal-500 valid:border-2 valid:border-teal-500"
                spellcheck="false"
                placeholder="Введите название города"
                required
              />
              <div
                class="separator h-px w-full flex items-center justify-center relative bg-slate-400 my-8 before:content-['или'] before:text-slate-400 before:px-4 before:bg-white before:text-lg"
              ></div>
              <button
                v-on:click="fetchWeatherWithGeo"
                class="text-white h-14 rounded-md text-md bg-teal-500 hover:bg-teal-700 w-full"
              >Получить по геолокации</button>
            </div>
          </section>
        </div>
        <div v-if="isActive">
          <section
            v-if="weather.cod != '404'"
            class="flex items-center justify-center flex-col mt-5"
          >
            <div v-html="weatherIcon(weather.weather[0].main)" class="my-4"></div>
            <div class="temp flex font-medium text-7xl">
              <span class="numb font-semibold">{{ Math.round(weather.main.temp) }}</span>
              <span class="deg block text-4xl mt-2 mr-1">°</span>C
            </div>
            <div
              class="weather my-4 mx-10 text-center text-xl font-semibold"
            >{{ weather.weather[0].main }}</div>
            <div class="flex text-lg px-8 text-center mb-8 justify-start">
              <box-icon type="solid" name="map" class="text-lg"></box-icon>
              <span>{{ weather.name }}, {{ weather.sys.country }}</span>
            </div>
            <div class="bottom-details flex w-full justify-between border-t border-t-slate-300">
              <div class="flex w-full items-center justify-center py-4">
                <box-icon type="solid" name="thermometer" class="text-lg text-teal-500"></box-icon>
                <div class="ml-1">
                  <span
                    class="flex font-medium text-lg mt-0 text-center"
                  >{{ Math.round(weather.main.feels_like) }} °C</span>
                  <p class="mt-0">Ощущается</p>
                </div>
              </div>
              <div class="flex w-full items-center justify-center py-4 border-l border-l-slate-300">
                <box-icon type="solid" name="droplet-half" class="text-lg text-teal-500"></box-icon>
                <div class="ml-1">
                  <span class="font-medium text-lg mt-0 text-center">{{ weather.main.humidity }} %</span>
                  <p class="mt-0">Влажность</p>
                </div>
              </div>
            </div>
          </section>
          <section
            v-else-if="weather.cod == '404'"
            class="flex items-center justify-center flex-col my-5"
          >
            <p
              class="p-4 bg-red-100 text-red-700 rounded font-semibold"
            >Введите правильное название города</p>
          </section>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'app',
  data() {
    return {
      api: '',
      api_key: '6b78e03a709725a5de19f3a252da8286',
      query: '',
      weather: {},
      isActive: false,
    }
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        this.api = `http://api.openweathermap.org/data/2.5/weather?q=${this.query}&units=metric&APPID=${this.api_key}`;
        this.fetchData(this.api);
      }
    },
    fetchWeatherWithGeo() {
      navigator.geolocation.getCurrentPosition(pos => {
        this.latitude = pos.coords.latitude;
        this.longitude = pos.coords.longitude;
        this.api = `https://api.openweathermap.org/data/2.5/weather?lat=${this.latitude}&lon=${this.longitude}&units=metric&appid=${this.api_key}`;
        this.fetchData(this.api);
      });
    },
    fetchData() {
      fetch(this.api)
      .then(response => {
        return response.json();
      }).then(this.weatherResult);
    },
    weatherResult(result) {
      this.weather = result;
      this.isActive = true;
    },
    returnForm() {
      this.isActive = false;
    },
    weatherIcon(weather) {
      let clouds = '<svg class="h-24 w-24 text-teal-500"  width="24" height="24" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">  <path stroke="none" d="M0 0h24v24H0z"/>  <path d="M7 18a4.6 4.4 0 0 1 0 -9h0a5 4.5 0 0 1 11 2h1a3.5 3.5 0 0 1 0 7h-12" /></svg>';
      let clear = '<svg class="h-24 w-24 text-teal-500"  viewBox="0 0 24 24"  fill="none"  stroke="currentColor"  stroke-width="2"  stroke-linecap="round"  stroke-linejoin="round">  <circle cx="12" cy="12" r="5" />  <line x1="12" y1="1" x2="12" y2="3" />  <line x1="12" y1="21" x2="12" y2="23" />  <line x1="4.22" y1="4.22" x2="5.64" y2="5.64" />  <line x1="18.36" y1="18.36" x2="19.78" y2="19.78" />  <line x1="1" y1="12" x2="3" y2="12" />  <line x1="21" y1="12" x2="23" y2="12" />  <line x1="4.22" y1="19.78" x2="5.64" y2="18.36" />  <line x1="18.36" y1="5.64" x2="19.78" y2="4.22" /></svg>';
      let snow = '<svg class="h-24 w-24 text-teal-500"  viewBox="0 0 24 24"  fill="none"  stroke="currentColor"  stroke-width="2"  stroke-linecap="round"  stroke-linejoin="round">  <path d="M20 17.58A5 5 0 0 0 18 8h-1.26A8 8 0 1 0 4 16.25" />  <line x1="8" y1="16" x2="8.01" y2="16" />  <line x1="8" y1="20" x2="8.01" y2="20" />  <line x1="12" y1="18" x2="12.01" y2="18" />  <line x1="12" y1="22" x2="12.01" y2="22" />  <line x1="16" y1="16" x2="16.01" y2="16" />  <line x1="16" y1="20" x2="16.01" y2="20" /></svg>';
      let mist = '<svg class="h-24 w-24 text-teal-500"  width="24" height="24" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">  <path stroke="none" d="M0 0h24v24H0z"/>  <path d="M5 5h3m4 0h9" />  <path d="M3 10h11m4 0h1" />  <path d="M5 15h5m4 0h7" />  <path d="M3 20h9m4 0h3" /></svg>';
      let wind = '<svg class="h-24 w-24 text-teal-500"  viewBox="0 0 24 24"  fill="none"  stroke="currentColor"  stroke-width="2"  stroke-linecap="round"  stroke-linejoin="round">  <path d="M9.59 4.59A2 2 0 1 1 11 8H2m10.59 11.41A2 2 0 1 0 14 16H2m15.73-8.27A2.5 2.5 0 1 1 19.5 12H2" /></svg>';
      if (weather == 'Clouds')
        return clouds;
      else if (weather == 'Clear')
        return clear;
      else if (weather == 'Snow')
        return snow;
      else if (weather == 'Mist')
        return mist;
      else if (weather == 'Wind')
        return wind;
      else
        return weather;
    }
  },
}
</script>