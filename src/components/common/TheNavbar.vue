<template>
    <header class="sticky top-0 z-10 bg-weather-primary shadow-lg">
        <nav class="container flex flex-col sm:flex-row justify-between items-center gap-4 text-white py-6">
            <router-link :to="{ name: 'home' }">
                <div class="flex items-center gap-3 flex-1">
                    <i class="fa-solid fa-sun text-2xl"></i>
                    <p class="text-2xl">The Local Weather</p>
                </div>
            </router-link>
            <div class="flex gap-3">
                <i @click="toggleModal" class="cursor-pointer fa-solid fa-circle-info text-2xl hover:text-weather-secondary duration-150"></i>
                <i @click="addCity" v-if="route.query.preview" class="cursor-pointer fa-solid fa-plus text-2xl hover:text-weather-secondary duration-150"></i>
            </div>
            <base-modal :showModal="showModal" @close-modal="toggleModal">
                <div class="text-black">
                    <h1 class="text-2xl mb-1">About:</h1>
                    <p class="mb-4">
                        The Local Weather allows you to track the current and future weather of cities of your choosing.
                    </p>
                    <h2 class="text-2xl">How it works:</h2>
                    <ol class="list-decimal list-inside mb-4">
                        <li>
                            Search for your city by entering the name into the search bar.
                        </li>
                        <li>
                            Select a city within the results, this will take yuo to the current weather for your selection
                        </li>
                        <li>
                            Track the city by clicking on the "+" icon in the top right. This will save th city to view at a later time on the home page.
                        </li>
                    </ol>
                    <h2 class="text-2xl">Removing a city</h2>
                    <p>
                        if you no longer with to track a city, simply select the city within the home page. At the bottom of the page, there will be an option to delete the city.
                    </p>
                </div>
            </base-modal>
        </nav>
    </header>
</template>

<script setup>
import BaseModal from "@/components/common/BaseModal.vue";
import {reactive, ref} from "vue";
import {uid} from "uid";
import {useRoute, useRouter} from "vue-router";
const showModal = ref(false)
const allSavedCities = ref([])
const route = useRoute()
const router = useRouter()
const addCity = () => {
    if (localStorage.getItem('savedCities')) {
        allSavedCities.value =  JSON.parse(localStorage.getItem('savedCities'))
    }
    const locationObj = {
        id: uid(),
        city: route.params.city,
        state: route.params.state,
        coords: {
            lng: route.query.lng,
            lat: route.query.lat
        }
    }

    allSavedCities.value.push(locationObj)
    localStorage.setItem('savedCities', JSON.stringify(allSavedCities.value))
    const query = Object.assign({}, route.query)
    delete query.preview;
    query.id = locationObj.id
    router.replace({ query })
}
const toggleModal = () => {
    showModal.value = !showModal.value
}
</script>

<style scoped lang="scss">

</style>