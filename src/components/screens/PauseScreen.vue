<template>
  <div>
    <Message :mainText="`This is the Pause Screen!`" :mainStyles="`text-3xl`" />

    <ReusableButton
      label="Play Again"
      :onClick="handleRemovePause"
      customClass="bg-green-500 text-lg py-2 px-4 mt-4"
    />
  </div>
</template>

<script setup>
import Message from "../main/Message.vue";
import ReusableButton from "../main/ReusableButton.vue";

const emit = defineEmits(["onRemovePause"]);

const handleRemovePause = () => {
  emit("onRemovePause");
};
</script>
