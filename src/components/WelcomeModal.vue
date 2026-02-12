<template>
  <transition name="fade">
    <div v-if="visible" class="overlay">
      <div class="modal">
        <h2>Привіт 💜</h2>
        <button class="start-btn" @click="start">
          Привіт ✨
        </button>
      </div>
    </div>
  </transition>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const emit = defineEmits<{
  (e: 'start'): void
}>()

const visible = ref(true)

const start = () => {
  visible.value = false
  emit('start')
}
</script>

<style scoped>
.overlay {
  position: fixed;
  inset: 0;
  background: rgba(15, 15, 20, 0.85);
  backdrop-filter: blur(6px);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 9999;
}

.modal {
  background: linear-gradient(135deg, #f3e8ff, #ffe4f1);
  padding: 40px;
  border-radius: 24px;
  text-align: center;
  width: 300px;
  box-shadow: 0 20px 40px rgba(0,0,0,0.3);
  animation: popIn 0.5s ease;
}

.start-btn {
  margin-top: 20px;
  padding: 12px 24px;
  border-radius: 30px;
  border: none;
  background: #ff69b4;
  color: white;
  font-size: 1rem;
  cursor: pointer;
  transition: 0.3s ease;
}

.start-btn:hover {
  transform: scale(1.08);
  background: #ff1493;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.4s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

@keyframes popIn {
  from {
    transform: scale(0.7);
    opacity: 0;
  }
  to {
    transform: scale(1);
    opacity: 1;
  }
}
</style>
