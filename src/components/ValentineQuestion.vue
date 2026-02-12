<template>
  <section class="question">
    <h2 class="title">Ти станеш моєю Валентинкою? 💌</h2>

    <div class="buttons">
      <button
        class="yes-btn"
        :class="{ grow: noAttempts >= 10 }"
        @click="accept"
      >
        Так 💖
      </button>

      <button
        v-if="noAttempts < 10"
        ref="noBtn"
        class="no-btn"
      >
        Ні 😢
      </button>
    </div>

    <transition name="pop">
      <div v-if="accepted" class="love-message">
        Я знав 😌❤️
      </div>
    </transition>

    <transition name="slide">
      <img
        v-if="noAttempts >= 10"
        class="side-image"
        :src="gruImg"
        alt="gru"
      />
    </transition>
  </section>
</template>

<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount } from 'vue'
import { nope, oya, gruImg } from '@/assets'

const accepted = ref(false)
const noBtn = ref<HTMLButtonElement | null>(null)

const noAttempts = ref(0)
const isMoving = ref(false)
const MOVE_DURATION_MS = 300
let unlockTimer: number | null = null

const RADIUS = 80
const MIN_MOVE_DISTANCE = 200
const SOUND_DELAY = 500

const yesSound = new Audio(oya)
yesSound.volume = 0.2

const escapeSound = new Audio(nope)
escapeSound.volume = 0.4

let lastSoundTime = 0

const accept = () => {
  accepted.value = true
  yesSound.currentTime = 0
  yesSound.play()
}

const getDistance = (
  x1: number,
  y1: number,
  x2: number,
  y2: number
) => {
  return Math.sqrt((x2 - x1) ** 2 + (y2 - y1) ** 2)
}

const playEscapeSound = () => {
  const now = Date.now()
  if (now - lastSoundTime > SOUND_DELAY) {
    escapeSound.currentTime = 0
    escapeSound.play()
    lastSoundTime = now
  }
}

const moveButton = (e: MouseEvent) => {
  if (!noBtn.value) return
  if (isMoving.value) return
  if (noAttempts.value >= 20) return

  const rect = noBtn.value.getBoundingClientRect()

  const btnCenterX = rect.left + rect.width / 2
  const btnCenterY = rect.top + rect.height / 2

  const distanceToMouse = getDistance(
    e.clientX,
    e.clientY,
    btnCenterX,
    btnCenterY
  )

  if (distanceToMouse < RADIUS) {
    isMoving.value = true
    noAttempts.value++

    const maxX = window.innerWidth - rect.width
    const maxY = window.innerHeight - rect.height

    let newX = 0
    let newY = 0
    let distanceFromCurrent = 0

    const currentX = rect.left
    const currentY = rect.top

    // Перевірка "мінімум 200px"
    do {
      newX = Math.random() * maxX
      newY = Math.random() * maxY
      distanceFromCurrent = getDistance(currentX, currentY, newX, newY)
    } while (distanceFromCurrent < MIN_MOVE_DISTANCE)

    // Применяємо стилі
    noBtn.value.style.position = 'fixed'
    noBtn.value.style.transition = `left ${MOVE_DURATION_MS}ms ease, top ${MOVE_DURATION_MS}ms ease`
    noBtn.value.style.left = `${newX}px`
    noBtn.value.style.top = `${newY}px`

    playEscapeSound()

    // ✅ Гарантований анлок навіть якщо transitionend не прийде
    if (unlockTimer) window.clearTimeout(unlockTimer)
    unlockTimer = window.setTimeout(() => {
      isMoving.value = false
    }, MOVE_DURATION_MS + 50)
  }
}

const onNoTransitionEnd = (ev: TransitionEvent) => {
  if (ev.propertyName === 'left' || ev.propertyName === 'top') {
    isMoving.value = false
  }
}

onMounted(() => {
  window.addEventListener('mousemove', moveButton)
  noBtn.value?.addEventListener('transitionend', onNoTransitionEnd)
})

onBeforeUnmount(() => {
  window.removeEventListener('mousemove', moveButton)
  noBtn.value?.removeEventListener('transitionend', onNoTransitionEnd)
  if (unlockTimer) window.clearTimeout(unlockTimer)
})
</script>




<style scoped>
.question {
  min-height: 40vh;
  height: 50%;
  background: linear-gradient(135deg, #1a1a1a, #4b0082);
  color: white;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 40px;
  text-align: center;
  overflow: hidden;
}

.title {
  font-size: 2rem;
  animation: float 3s ease-in-out infinite;
}

.buttons {
  display: flex;
  gap: 30px;
}

button {
  padding: 14px 32px;
  border-radius: 30px;
  border: none;
  font-size: 1rem;
  cursor: pointer;
  transition: all 0.3s ease;
}

.yes-btn {
  background: #ff69b4;
  color: white;
  box-shadow: 0 10px 20px rgba(255, 105, 180, 0.4);
}

.yes-btn:hover {
  transform: scale(1.1);
  background: #ff1493;
}

.no-btn {
  background: #98fb98;
  color: #1a1a1a;
}

.love-message {
  font-size: 2rem;
  color: #ffb6f9;
  margin-top: 15px;
}

@keyframes float {
  0% { transform: translateY(0px); }
  50% { transform: translateY(-8px); }
  100% { transform: translateY(0px); }
}

.pop-enter-active {
  animation: pop 0.6s ease;
}

@keyframes pop {
  0% { transform: scale(0.5); opacity: 0; }
  100% { transform: scale(1); opacity: 1; }
}
.yes-btn.grow {
  transform: scale(2.5);
  font-size: 1.8rem;
  box-shadow: 0 0 40px rgba(255, 105, 180, 0.7);
  transition: all 0.4s ease;
}

/* --- КАРТИНКА СПРАВА --- */
.side-image {
  position: absolute;
  right: 40px;
  bottom: 0;
  width: 280px;
  border-radius: 20px;
  animation: slideIn 0.6s ease forwards;
}

/* --- Анімація появи справа --- */
.slide-enter-active {
  transition: all 0.6s ease;
}

.slide-enter-from {
  transform: translateX(100%);
  opacity: 0;
}

.slide-enter-to {
  transform: translateX(0);
  opacity: 1;
}

@keyframes slideIn {
  from {
    transform: translateX(100%);
    opacity: 0;
  }
  to {
    transform: translateX(0);
    opacity: 1;
  }
}
</style>
