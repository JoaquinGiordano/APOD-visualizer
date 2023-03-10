<script setup>
import APODContainer from '../components/APODContainer.vue';
</script>

<template>
  <div class="home">
    <APODContainer class="apod-container" />
  </div>
</template>

<style scoped>
.home {
  display: flex;
}

@media screen and (min-width: 730px) {
  .apod-container {
    width: 100%;
    margin: 40px;
    box-shadow: #cccccc 0px 0px 10px 0px;
  }
}
</style>
