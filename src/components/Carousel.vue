<template>
  <div class="main-container">
    <div class="carousel">
      <button @click="prevSlide">Prev</button>
      <div class="carousel-container">
        <div class="carousel-box-container" :style="itemStyle">
          <div
            v-for="(item, index) in carouselItems"
            :key="index"
            class="carousel-slide"
            :style="userSpecifiedWidth"
          >
            <img :src="item" alt="" class="img" />
          </div>
        </div>
      </div>
      <button @click="nextSlide">Next</button>
    </div>
    <div class="dots-container">
      <span
        v-for="(item, index) in carouselItems"
        :key="index"
        class="dot"
        :class="currentSlide === index && 'dot-active'"
        @click="goToSlide(index)"
      ></span>
    </div>
  </div>
</template>

<script setup>
import { computed, defineProps, onMounted, ref } from "vue";

const props = defineProps({
  carouselItems: {
    type: Array,
    required: true,
  },
  slideWidth: {
    type: Number,
    required: false,
  },
  autoplay: {
    type: Boolean,
    required: false,
    default: false,
  },
  autoPlayDelay: {
    type: Number,
    default: 2000,
  },
  slidesToScroll: {
    type: Number,
    default: 2,
  },
  width: {
    type: Number,
    default: 300,
  },
  slidesToShow: {
    type: Number,
    default: 4,
  },
  pauseHover: {
    type: Boolean,
    default: true,
  },
});

const currentSlide = ref(0);
let autoplayId = null;

const itemStyle = computed(() => ({
  width: `${
    props.carouselItems.length * (props.width || props.slidesToShow * 100)
  }px`,
  transform: `translateX(-${
    currentSlide.value * (props.width || props.slideWidth)
  }px)`,
  transition: "transform 0.5s ease",
}));

const userSpecifiedWidth = computed(() => ({
  width: `${props.width}px`,
}));

// const slideSize = computed(() => {
//   console.log(el.offsetWidth); slides = 4, width = 300, available = 1400px
//   width: `${calc(100%  props.slidesToShow - 1)}`,
//   console.log((1200 * (props.slidesToShow - 1))) / props.slidesToShow;
// });

const nextSlide = () => {
  // console.log(props.width || props.slidesToShow);
  if (autoplayId) {
    stopAutoPlay();
  }

  currentSlide.value =
    currentSlide.value >= props.carouselItems.length - props.slidesToScroll
      ? (currentSlide.value = 0)
      : (currentSlide.value += props.slidesToScroll);

  //   console.log(currentSlide.value);
};

const prevSlide = () => {
  if (autoplayId) {
    stopAutoPlay();
  }

  currentSlide.value =
    currentSlide.value <= 0
      ? (currentSlide.value = props.carouselItems.length - props.slidesToScroll)
      : (currentSlide.value -= props.slidesToScroll);

  // console.log(currentSlide.value);
};

const goToSlide = (idx) => {
  currentSlide.value = idx;
};

const loop = () => {
  currentSlide.value =
    currentSlide.value >= props.carouselItems.length - props.slidesToScroll
      ? (currentSlide.value = 0)
      : (currentSlide.value += props.slidesToScroll);
};

const startAutoPlay = () => {
  if (props.autoplay) {
    autoplayId = setInterval(() => {
      loop();
    }, props.autoPlayDelay);
    // console.log(autoplayId);
  }
};

const stopAutoPlay = function () {
  if (autoplayId) {
    clearInterval(autoplayId);
  }
  console.log("Stop Auto Play");
};

onMounted(() => {
  startAutoPlay();
});

// console.log(props.carouselItems);
</script>

<style lang="scss" scoped>
.carousel {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-left: auto;
  margin-right: auto;
  /* width: 100%; */
  min-width: 140rem;
  margin-top: 5rem;
}

.carousel-container {
  overflow: hidden;
  width: 100%;
  
}

.carousel-box-container {
  overflow: hidden;
  display: flex;
  width: 100%;
  // min-width: 120rem;
  // position: absolute;
  // top: 10%;
  // left: 5%;
  // right: 5%;
}

.carousel-slide {
  margin-right: 10px;
  // width: 400px;
  /* width: auto; */
  /* width: calc((100% - 10px * 4) / 5); */
}
// .col-1-of-4 {
//   // width: 330px;
//   // width: 260px;
//   // width: 210px;
//   // width: 100%;
//   width: calc((100% - 1rem * 3) / 4);
// }

.img {
  width: 100%;
}

.dots-container {
  display: flex;
  align-items: center;
  justify-content: center;
  margin-top: 15px;
  position: absolute;
  left: 50%;
  transform: translate(-50%, -50%);
}
.dot {
  height: 10px;
  width: 10px;
  border-radius: 50%;
  border: 1px solid rgb(60, 58, 58);
  cursor: pointer;
}
.dot:not(:last-child) {
  margin-right: 10px;
}
.dot-active {
  background-color: black;
}
</style>
