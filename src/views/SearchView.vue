<template>
  <div
    class="flex flex-col items-center justify-center min-h-screen bg-gray-100 p-6 relative overflow-hidden"
  >
    <NumstoreLogo />
    <SearchBar />
    <NSButton />
  </div>
</template>

<style>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
}
</style>
<script lang="ts" setup>
import NumstoreLogo from '@/components/NumstoreLogo.vue'
import SearchBar from '@/components/SearchBar.vue'
import NSButton from '@/components/NSButton.vue'
</script>
