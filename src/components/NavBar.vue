<script setup>
import { ref } from 'vue'
import { RouterLink } from 'vue-router'

const showMenu = ref(false)

const toggleMenu = () => {
  showMenu.value = !showMenu.value
}
</script>

<template>
  <header class="bg-[#0b6a75] shadow-md">
    <div class="container md:mx-auto px-2 flex justify-between items-center py-4">
      <div class="text-white text-2xl">
        <RouterLink to="/" class="text-white">TodoApp</RouterLink>
      </div>
      <div class="hidden md:flex items-center gap-7">
        <!-- Navigation links for larger screens -->
        <RouterLink to="/about" class="text-white">About</RouterLink>
        <RouterLink to="/" class="text-white">Todo</RouterLink>
      </div>
      <div class="flex items-center">
        <!-- Hamburger menu icon -->
        <button @click="toggleMenu" class="text-white block md:hidden">
          <svg class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M4 6h16M4 12h16m-7 6h7"
            ></path>
          </svg>
        </button>
      </div>
    </div>
  </header>
  <!-- Navigation links for small screens -->
  <div v-if="showMenu" class="bg-[#0b6a75] md:hidden py-4">
    <div class="container mx-auto flex flex-col items-center">
      <RouterLink to="/about" class="text-white block my-2">About</RouterLink>
      <RouterLink to="/" class="text-white block my-2">Todo</RouterLink>
    </div>
  </div>
</template>
