<template>
  <header class="text-gray-400 bg-gray-900 body-font">
    <div class="container mx-auto flex flex-wrap p-5 flex-col md:flex-row items-center">
      <a class="flex title-font font-medium items-center text-white mb-4 md:mb-0">
        <span class="text-xl">Vue + Tesseract.js</span>
      </a>
      <nav class="md:ml-auto flex flex-wrap items-center text-base justify-center">
        <RouterLink :to="{ name: 'Home' }" class="mr-5 hover:text-white" active-class="text-white"
          >Home</RouterLink
        >
        <RouterLink :to="{ name: 'About' }" class="mr-5 hover:text-white" active-class="text-white"
          >About</RouterLink
        >
      </nav>
    </div>
  </header>
</template>

<script setup>
import { RouterLink } from 'vue-router'
</script>
