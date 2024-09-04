<template>
  <Carousel
    :carousel-items="carouselItems"
    :slidesToShow="4"
    :slide-width="300"
    :width="300"
    :autoplay="true"
    :autoPlayDelay="2000"
    :slidesToScroll="1"
    :pauseHover="true"
  />
</template>

<script setup>
import { ref } from "vue";
import Carousel from "./components/Carousel.vue";

const carouselItems = ref([
  "https://picsum.photos/id/1032/300/300",
  "https://picsum.photos/id/1033/300/300",
  "https://picsum.photos/id/1035/300/300",
  "https://picsum.photos/id/1036/300/300",
  "https://picsum.photos/id/1037/300/300",
  "https://picsum.photos/id/1038/300/300",
  "https://picsum.photos/id/1039/300/300",
  "https://picsum.photos/id/1040/300/300",
  "https://picsum.photos/id/1041/300/300",
  "https://picsum.photos/id/1042/300/300",
]);
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
html {
  font-size: 62.5%;
  /* This defines what 1rem is 1rem === 10px */
}
</style>
