<template>
  <header class="container mx-auto p-6">
    <h1 class="text-4xl font-bold">{{ props.title }}</h1>
  </header>
</template>

<script setup>
const props = defineProps({
  title: String,
});
</script>
