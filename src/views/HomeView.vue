<template>
  <main class="container text-white">
      <div class="pt-4 mb-8 relative">
          <input
              autofocus
              @input="getSearchResults"
              class="py-2 px-1 w-full
                  bg-transparent border-b
                  focus:shadow-[0_1px_0_0_#004E71]
                  focus:outline-none
                  focus:border-weather-secondary"
              type="text"
              placeholder="Search for a city"
              v-model="searchInput">
          <ul class="absolute bg-weather-secondary text-white w-full shadow-md py-2 px-1 top-16"
              v-if="searchResults">
              <p v-if="searchError">
                  Sorry, something went wrong, please try again.
              </p>
              <p v-else-if="!searchError && searchResults.length === 0">
                  No results match your query, try a different term.
              </p>
              <template v-else>
                  <li
                      v-for="result in searchResults"
                      :key="result.id"
                      class="hover:bg-weather-primary py-2 cursor-pointer"
                      @click="showCityInfo(result)"
                  >
                      {{ result.place_name }}
                  </li>
              </template>
          </ul>
      </div>
      <div class="flex flex-col gap-4">
          <suspense>
              <city-list></city-list>
              <template #fallback>
                  <city-card-animation></city-card-animation>
              </template>
          </suspense>
      </div>
  </main>
</template>

<script setup>
import {ref} from "vue";
import axios from "axios";
import {useRouter} from "vue-router";
import CityList from "@/components/common/CityList.vue";
import CityCardAnimation from "@/components/common/animation/CityCardAnimation.vue";
const searchInput = ref('')
const searchResults = ref(null)
const inputTimeout = ref(null)
const searchError = ref(false)
const router = useRouter()
const showCityInfo = (searchresult) => {
    const [ city, state ] = searchresult.place_name.split(',')
    console.log(city)
    console.log(state)
    router.push({
        name: 'city',
        params: { city: city, state: state ? state.replaceAll(" ", "", ",") : '' },
        query: {
            lng: searchresult.geometry.coordinates[0],
            lat: searchresult.geometry.coordinates[1],
            preview: true
        }
    })
}
const token = 'pk.eyJ1IjoiZG90dGVyIiwiYSI6ImNsaGt2NXBucDB2czYzY254dHQ0NGNzcDIifQ.W0mEzqD24Kt7oex1HaN5ng'
const getSearchResults = () => {
    inputTimeout.value = setTimeout(async ()=>{
      if(searchInput.value !== '') {
          try {
              const { data } =  await axios.get(
                  `https://api.mapbox.com/geocoding/v5/mapbox.places/${searchInput.value}.json?access_token=${token}&types=place`
              )
              searchResults.value = data.features
          } catch (e) {
            searchError.value = true
          }
          return;
      }
      searchResults.value = null
  }, 300)
}
</script>
