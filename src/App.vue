<template>
  <div class="flex flex-col min-h-screen font-Roboto bg-weather-primary">
      <the-navbar></the-navbar>
      <router-view v-slot="{ Component }">
          <transition name="page" mode="out-in">
            <component :is="Component"></component>
          </transition>
      </router-view>
  </div>
</template>

<script setup>

import TheNavbar from "@/components/common/TheNavbar.vue";
</script>

<style lang="scss">
.page-enter-active,
.page-leave-active {
  transition: 800ms ease all;
}

.page-enter-from,
.page.leave-to {
  opacity: 0;
}
</style>