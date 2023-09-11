<template>
  <div :class="props.commonStyles">
    {{ props.preText }}
    <span :class="props.mainStyles">{{ props.mainText }}</span>
    {{ props.afterText }}
  </div>
</template>

<script setup>
const props = defineProps({
  preText: String,
  mainText: null,
  afterText: String,
  commonStyles: String,
  mainStyles: String,
});
</script>
