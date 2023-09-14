<template>
  <button :class="props.customClass" @click="props.onClick">
    <span v-if="props.img">
      <img :src="props.img" :alt="props.alt" class="w-full" />
    </span>
    <span v-else>{{ props.label }}</span>
  </button>
</template>

<script setup>
const props = defineProps({
  label: String,
  onClick: Function,
  customClass: String,
  img: String,
  alt: String,
});
</script>
