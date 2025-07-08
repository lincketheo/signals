<script lang="ts" setup>
import {RouterView} from 'vue-router'
</script>

<template>
  <main class="flex flex-col min-h-screen mx-auto bg-red-500">
    <!-- Header -->
    <header class="h-10 bg-blue-500 m-4 flex justify-end space-x-6 items-center">
      <a class="flex flex-col items-center" href="/create">
        <i class="fa-solid fa-plus items-center"></i>
        <h1>Create</h1>
      </a>

      <a class="flex flex-col items-center" href="/search">
        <i class="fa-solid fa-search"></i>
        <h1>Search</h1>
      </a>

      <a class="flex flex-col items-center" href="/account">
        <i class="fa-solid fa-user"></i>
        <h1>Account</h1>
      </a>
    </header>

    <RouterView class="flex-1"/>
  </main>
</template>

<style scoped>

</style>
