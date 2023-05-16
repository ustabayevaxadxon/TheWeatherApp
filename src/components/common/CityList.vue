<template>
  <div v-for="city in allSavedCities" :key="city.id">
      <city-card :city="city" @click="goToCityDetails(city)"></city-card>
  </div>
  <p v-if="allSavedCities.length === 0">
      No locations added. To start tracking a location, search in  the field above.
  </p>
</template>

<script setup>
import {ref} from "vue";
import axios from "axios";
import CityCard from "@/components/common/CityCard.vue";
import {useRouter} from "vue-router";
const router = useRouter()
const allSavedCities = ref([])
const getSavedCities = async () => {
    if (localStorage.getItem('savedCities')) {
        allSavedCities.value =  JSON.parse(localStorage.getItem('savedCities'))
        const request = []
        allSavedCities.value.forEach(city => {
            request.push(
                axios.get(`https://api.openweathermap.org/data/2.5/weather?lat=${city.coords.lat}&lon=${city.coords.lng}&appid=7efa332cf48aeb9d2d391a51027f1a71&units=metric`)
            )
        })

        const data = await Promise.all(request)

        await new Promise(res => setTimeout(res,1000))

        data.forEach((value, index) => {
            allSavedCities.value[index].weather = value.data
        })
    }
}
const goToCityDetails = (city) => {
    router.push({
        name: 'city',
        params: { city: city.city },
        query: { id: city.id, lng: city.coords.lng, lat: city.coords.lat }
    })
}
await getSavedCities()
</script>

<style scoped lang="scss">

</style>