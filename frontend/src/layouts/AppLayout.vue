<template>
  <div class="app_layout">
    <app-layout-header />
    <slot />
  </div>
</template>

<script setup>
import AppLayoutHeader from "./AppLayoutHeader.vue";
</script>

<style lang="scss" scoped>
.app__layout {
  display: flex;
  flex-direction: column;
  height: 100vh;
}
</style>
