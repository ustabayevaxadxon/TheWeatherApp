<template>
    <section>
        <div class="flex items-center px-3 py-6 bg-weather-secondary bg-opacity-60 hover:bg-opacity-30 rounded-md shadow-md cursor-pointer">
            <div class="flex flex-col flex-1">
                <h2 class="text-2xl">{{ city.city }}</h2>
                <h3 class="text-xl">{{ city.state }}</h3>
            </div>
            <div class="flex flex-col gap-2">
                <p class="text-3xl self-end">
                    {{ Math.round(city.weather.main.temp) }}&deg;
                </p>
                <div class="flex gap-2">
                    <span class="text-xs">H: {{ Math.round(city.weather.main.temp_max) }}&deg;</span>
                    <span class="text-xs">L: {{ Math.round(city.weather.main.temp_min) }}&deg;</span>
                </div>
            </div>
        </div>
    </section>
</template>

<script setup>
defineProps({
    city: {
        type: Object,
        default: () => {}
    }
})
</script>

<style scoped lang="scss">

</style>