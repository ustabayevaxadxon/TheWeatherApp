<template>
  <div>
      <suspense>
          <async-city-view></async-city-view>
          <template #fallback>
             <city-view-animation></city-view-animation>
          </template>
      </suspense>
  </div>
</template>

<script setup>

import AsyncCityView from "@/components/common/AsyncCityView.vue";
import CityViewAnimation from "@/components/common/animation/CityViewAnimation.vue";
</script>

<style scoped lang="scss"></style>