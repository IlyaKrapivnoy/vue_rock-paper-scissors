<template>
  <div class="bg-gray-700 text-white text-center min-h-screen flex flex-col">
    <Header :title="`Rock, Paper, Scissors!`" />
    <div class="flex-1">
      <slot />
    </div>
    <Footer class="mt-auto" />
  </div>
</template>

<script setup>
import Header from "./Header.vue";
import Footer from "./Footer.vue";
</script>
