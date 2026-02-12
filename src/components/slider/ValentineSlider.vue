<template>
  <section class="slider">
    <transition-group name="fade" tag="div" class="slider-wrapper">
      <img
        v-for="(photo, index) in photos"
        v-show="index === currentIndex"
        :key="photo"
        :src="photo"
        class="slide"
      />
    </transition-group>
  </section>
</template>

<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount } from 'vue'

const props = defineProps<{
  photos: string[]
  interval?: number
}>()

const currentIndex = ref(0)
let timer: number | undefined

const startSlider = () => {
  timer = window.setInterval(() => {
    currentIndex.value =
      (currentIndex.value + 1) % props.photos.length
  }, props.interval ?? 4000)
}

onMounted(startSlider)
onBeforeUnmount(() => {
  if (timer) clearInterval(timer)
})
</script>

<style scoped>
.slider {
  width: 100%;
  height: 50vh;
  overflow: hidden;
  position: relative;
  background: linear-gradient(135deg, #f3e8ff, #ffe4f1);
  display: flex;
  align-items: center;
  justify-content: center;
  padding-bottom: 24px;
  padding-top: 24px;
}

.slider-wrapper {
  position: relative;
  width: 80%;
  height: 100%;
}

.slide {
  position: absolute;
  width: 100%;
  height: 100%;
  object-fit: contain;
  border-radius: 24px;
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.2);
  transition: transform 1.2s ease;
}

.slide:hover {
  transform: scale(1.03);
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 1.2s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
