<template>
  <div class="hearts-container">
    <span
      v-for="heart in hearts"
      :key="heart.id"
      class="heart"
      :style="heart.style"
    >
      💖
    </span>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount } from 'vue'

interface Heart {
  id: number
  style: Record<string, string>
}

const hearts = ref<Heart[]>([])
let intervalId: number | null = null
let heartId = 0

const createHeart = () => {
  const size = Math.random() * 20 + 15
  const left = Math.random() * 100
  const duration = Math.random() * 3 + 4
  const delay = Math.random() * 2

  const colors = ['#ff69b4', '#d946ef', '#f472b6', '#a78bfa']

  hearts.value.push({
    id: heartId++,
    style: {
      left: `${left}%`,
      fontSize: `${size}px`,
      animationDuration: `${duration}s`,
      animationDelay: `${delay}s`,
      color: colors[Math.floor(Math.random() * colors.length)],
    },
  })

  setTimeout(() => {
    hearts.value = hearts.value.slice(1)
  }, (duration + delay) * 1000)
}

onMounted(() => {
  intervalId = window.setInterval(createHeart, 350)
})

onBeforeUnmount(() => {
  if (intervalId) clearInterval(intervalId)
})
</script>

<style scoped>
.hearts-container {
  position: fixed;
  inset: 0;
  pointer-events: none;
  overflow: hidden;
  z-index: 5;
}

.heart {
  position: absolute;
  top: -30px;
  animation-name: fall;
  animation-timing-function: linear;
  animation-fill-mode: forwards;
  opacity: 0.85;
}

@keyframes fall {
  0% {
    transform: translateY(0) rotate(0deg);
    opacity: 1;
  }
  100% {
    transform: translateY(110vh) rotate(360deg);
    opacity: 0;
  }
}
</style>
