<script setup>
import SpaceCapsules from "@/components/SpaceCapsules/SpaceCapsules.vue";
</script>
<template>
  <div class="about">
    <h1>Launches</h1>
    <SpaceCapsules />
  </div>
</template>

<style>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    align-items: center;
    flex-direction: column;
  }
}
</style>
