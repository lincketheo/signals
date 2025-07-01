<script lang="ts" setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import NumstoreLogo from '@/components/NumstoreLogo.vue'

const router = useRouter()
const createBtn = ref<HTMLButtonElement | null>(null)

function onCreateClick(e: MouseEvent) {
  const btn = createBtn.value
  if (!btn) return

  const rect = btn.getBoundingClientRect()
  const x = e.clientX - rect.left
  const y = e.clientY - rect.top

  const ripple = document.createElement('span')
  ripple.className = 'ripple'
  ripple.style.left = `${x}px`
  ripple.style.top = `${y}px`

  btn.appendChild(ripple)

  ripple.addEventListener('animationend', () => {
    ripple.remove()
    router.push('/create')
  })
}
</script>

<template>
  <main>
    <div
      class="flex flex-col items-center justify-center min-h-screen bg-gray-100 p-6 relative overflow-hidden"
    >
      <NumstoreLogo />

      <div class="flex gap-12">
        <RouterLink to="/search">
          <button
            class="w-28 h-28 rounded-full bg-blue-600 text-white text-lg flex flex-col items-center justify-center shadow-lg hover:bg-blue-700 transition"
          >
            <i class="fas fa-search text-2xl"></i>
            <span class="mt-2">Search</span>
          </button>
        </RouterLink>
        <RouterLink to="/create">
          <button
            ref="createBtn"
            class="relative overflow-hidden w-28 h-28 rounded-full bg-green-600 text-white text-lg flex flex-col items-center justify-center shadow-lg hover:bg-green-700 transition"
            @click="onCreateClick"
          >
            <i class="fas fa-plus text-2xl"></i>
            <span class="mt-2">Create</span>
          </button>
        </RouterLink>
      </div>
    </div>
  </main>
</template>

<style scoped>
.ripple {
  position: absolute;
  width: 20px;
  height: 20px;
  background-color: rgba(255, 255, 255, 0.6);
  border-radius: 50%;
  pointer-events: none;
  transform: translate(-50%, -50%) scale(1);
  animation: ripple-expand 600ms ease-out;
}

@keyframes ripple-expand {
  to {
    transform: translate(-50%, -50%) scale(15);
    opacity: 0;
  }
}
</style>
