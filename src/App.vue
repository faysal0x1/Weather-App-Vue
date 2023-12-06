<template>
  <div
    class="flex flex-col md:max-w-full min-h-screen font-Roboto bg-weather-primary"
  >
    <SiteNavigation />
    <RouterView />
  </div>
</template>

<script setup>
import { RouterView } from "vue-router";
import SiteNavigation from "./components/SiteNavigation.vue";
</script>

<style lang="scss" scoped></style>