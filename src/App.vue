<template>
<div class="valentin-container">
  <WelcomeModal @start="handleStart" />

  <FallingHearts />

  <ValentineSlider :photos="photos" />
  <ValentineQuestion />
</div>
</template>

<script setup lang="ts">
import { ValentineSlider, ValentineQuestion, WelcomeModal, FallingHearts } from './components'
import { img1, img2, img3, img4, img5, img6, img7, img8, img9, img10, img11, img12, img13, img14, img15, img16, img17, ambients } from './assets'

const photos = [
  img1,
  img2,
  img3,
  img4,
  img5,
  img6,
  img7,
  img8,
  img9,
  img10,
  img11,
  img12,
  img13,
  img14,
  img15,
  img16,
  img17,
]

const ambient = new Audio(ambients)
ambient.loop = true
ambient.volume = 0.15

const handleStart = () => {
  ambient.play().catch(() => {})
}
</script>

<style lang="scss" scoped>

  .valentin-container {
    display: flex;
    flex-direction: column;
    gap: 12px;
    background-color: #ffe4f1;
    height: 100%;
    overflow-x:  hidden;
  }
</style>
