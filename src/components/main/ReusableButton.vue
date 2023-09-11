<template>
  <button :class="props.customClass" @click="props.onClick">
    {{ props.label }}
  </button>
</template>

<script setup>
const props = defineProps({
  label: String,
  onClick: Function,
  customClass: String,
});
</script>
