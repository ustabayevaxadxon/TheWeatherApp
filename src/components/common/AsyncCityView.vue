<template>
  <div class="flex flex-col flex-1 items-center">
      <div v-if="route.query.preview" class="text-white p-4 bg-weather-secondary w-full text-center">
          <p class="container">
              You are currently previewing this city, click the "+" icon to start tracking this city.
          </p>
      </div>
      <div class="grid grid-cols-1 min-[400px]:grid-cols-2 text-center items-center text-white py-12">
          <div class="min-[400px]:mr-12 px-4  text-center">
              <h1 class="text-4xl mb-2">{{ route.params.city }}</h1>
              <p class="text-sm mb-4">
                  {{new Date(data.currentTime).toLocaleDateString("en-us", {weekday: "short", day: "2-digit", month: "long",})}}
                  {{new Date(data.currentTime).toLocaleTimeString("en-us", {timeStyle: "short",})}}
              </p>
              <p class="text-7xl min-[512px]:text-8xl mb-4">
                  {{ Math.round(data.current.temp) }}&deg;
              </p>
          </div>
          <div class="flex flex-col items-center px-4 text-center">
              <p>Feels like {{ Math.round(data.current.feels_like) }}&deg</p>
              <p class="capitalize">{{ data.current.weather[0].description }}</p>
              <img class="w-[150px] h-auto" :src="`https://openweathermap.org/img/wn/${data.current.weather[0].icon}@4x.png`" alt="">
          </div>
      </div>
      <hr class="border-white border-opacity-10 border w-full">
      <div class="max-w-screen-md w-full py-12">
          <div class="mx-8 text-white">
              <h2 class="mb-4">Hourly Weather</h2>
              <div class="flex gap-10 overflow-x-scroll">
                  <div
                      class="flex flex-col gap-4 items-center"
                      v-for="hourlyData in data.hourly" :key="hourlyData.dt">
                      <p class="whitespace-nowrap text-md">
                          {{ new Date(hourlyData.currentTime).toLocaleTimeString('en-us', { hour: 'numeric' }) }}
                      </p>
                      <img class="w-auto h-[50px] object-cover" :src="`https://openweathermap.org/img/wn/${hourlyData.weather[0].icon}@4x.png`" alt="">
                      <p class="text-xl">{{ Math.round(hourlyData.temp) }}&deg</p>
                  </div>
              </div>
          </div>
      </div>
      <hr class="border-white border-opacity-10 border w-full">
      <div class="max-w-screen-md w-full py-12">
          <div class="mx-8 text-white">
              <h2 class="mb-4">7 Day Forecast</h2>
              <div
                  class="mb-2 border-b flex justify-between items-center"
                  v-for="dailyData in data.daily" :key="dailyData.dt"
              >
                  <p class="flex-1">{{ new Date(dailyData.dt * 1000).toLocaleDateString("en-us", { weekday: 'long' }) }}</p>
                  <img class="[50px] h-[50px] object-cover" :src="`https://openweathermap.org/img/wn/${dailyData.weather[0].icon}@4x.png`" alt="">
                  <div class="flex gap-2 flex-1 flex-col items-end justify-end">
                      <p class="w-[50px]">H: {{ Math.round(dailyData.temp.max) }}&deg;</p>
                      <p class="w-[50px]">L: {{ Math.round(dailyData.temp.min) }}&deg</p>
                  </div>
              </div>
          </div>
      </div>
      <div v-if="!route.query.preview" @click="removeCity" class="flex items-center gap-2 my-6 text-white cursor-pointer duration-150 hover:text-red-500">
          <i class="fa-solid fa-trash"></i>
          <p>Remove City</p>
      </div>
  </div>
</template>

<script setup>
import {useRoute, useRouter} from "vue-router";
import axios from "axios";
const route = useRoute()
const router = useRouter()
const getWeatherData = async () => {
    try {
        const { data } = await axios.get(
            `https://api.openweathermap.org/data/2.5/onecall?lat=${route.query.lat}&lon=${route.query.lng}&exclude={part}&appid=7efa332cf48aeb9d2d391a51027f1a71&units=metric`
        )
        const localOffset = new Date().getTimezoneOffset() * 60000;
        const utc = data.current.dt * 1000 + localOffset;
        data.currentTime = utc + 1000 * data.timezone_offset;
        data.hourly.forEach((hour) => {
            const utc = hour.dt * 1000 + localOffset;
            hour.currentTime = utc + 1000 * data.timezone_offset;
        });
        await new Promise(res => {setTimeout(res, 1000)})
        return data

    } catch (e) {
        console.log(e)
    }
}
const data = await getWeatherData()
const removeCity = () => {
    const cities = JSON.parse(localStorage.getItem("savedCities"))
    const updatedCities = cities.filter(city => city.id !== route.query.id)
    localStorage.setItem("savedCities", JSON.stringify(updatedCities))
    router.push({
        name: 'home'
    })
}
</script>

<style scoped lang="scss">
</style>