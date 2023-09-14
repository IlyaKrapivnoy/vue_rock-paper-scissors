<template>
  <div>
    <Message
      :preText="`You picked`"
      :mainText="props.choice"
      :commonStyles="`text-3xl mb-4`"
      :mainStyles="`text-pink-500`"
    />

    <Message
      :preText="`The computer picked`"
      :mainText="props.computerChoice"
      :commonStyles="`text-3xl mb-4`"
      :mainStyles="`text-green-500`"
    />

    <Message :mainText="props.verdict" :mainStyles="`text-6xl mb-12`" />

    <ReusableButton
      label="Reset"
      :onClick="handleResetRound"
      customClass="bg-pink-500 text-lg py-2 px-4 mt-12"
    />
  </div>
</template>

<script setup>
import ReusableButton from "../main/ReusableButton.vue";
import Message from "../main/Message.vue";

const emit = defineEmits(["onResetRound"]);

const props = defineProps({
  choice: String,
  computerChoice: String,
  verdict: String,
});

const handleResetRound = () => {
  emit("onResetRound");
};
</script>
