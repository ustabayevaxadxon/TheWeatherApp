<template>
  <div class="flex items-center px-3 py-6 bg-weather-secondary bg-opacity-60 hover:bg-opacity-30 rounded-md shadow-md">
      <div class="flex flex-col flex-1 gap-2">
          <animation class="max-w-[50%]"></animation>
          <animation class="max-w-[40%]"></animation>
      </div>
      <div class="flex flex-col flex-1 gap-2">
          <animation class="max-w-[50px] w-full"></animation>
          <animation class="max-w-[75px] w-full"></animation>
      </div>
  </div>
</template>

<script setup>

import Animation from "@/components/common/animation/Animation.vue";
</script>

<style scoped lang="scss">

</style>
<script setup>
</script>