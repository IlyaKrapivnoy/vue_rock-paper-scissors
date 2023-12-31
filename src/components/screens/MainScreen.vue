<template>
  <div>
    <div v-if="!choice" class="flex items-center justify-center -mx-6">
      <ReusableButton
        v-for="(button, index) in playButtons"
        :key="index"
        :label="button.label"
        :onClick="button.onClick"
        :customClass="button.customClass"
        :img="button.img"
        :alt="button.alt"
      />
    </div>

    <div v-else>
      <ChoiceScreen
        @onResetRound="resetRound"
        :choice="choice"
        :computerChoice="computerChoice"
        :verdict="verdict"
      />
    </div>

    <Message
      :mainText="`${wins} : ${draws} : ${losses}`"
      :commonStyles="`mt-12 text-3xl mb-4`"
    />

    <Message
      :preText="`Win rate:`"
      :mainText="`${Math.round(winPercentage)}%`"
      :commonStyles="`text-lg`"
    />
    <ReusableButton
      label="Start Over"
      :onClick="startOver"
      customClass="bg-red-500 text-lg py-2 px-4 mt-4 w-[160px]"
    />

    <ReusableButton
      label="Pause"
      :onClick="showPause"
      customClass="bg-blue-500 text-lg py-2 px-4 mt-4 w-[160px]"
    />
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from "vue";
import ReusableButton from "../main/ReusableButton.vue";
import Message from "../main/Message.vue";
import ChoiceScreen from "../screens/ChoiceScreen.vue";
import { outcomes } from "../../data/outcomes.js";

const wins = ref(0);
const draws = ref(0);
const losses = ref(0);

const choice = ref("");
const computerChoice = ref("");
const verdict = ref("");

const playButtons = [
  {
    onClick: () => play("rock"),
    img: "./src/assets/RockIcon.svg",
    alt: "Rock",
    customClass:
      "bg-white rounded-full shadow-lg w-64 p-12 mx-6 transition-colors duration-300 hover:bg-pink-500",
  },
  {
    onClick: () => play("paper"),
    img: "./src/assets/PaperIcon.svg",
    alt: "Paper",
    customClass:
      "bg-white rounded-full shadow-lg w-64 p-12 mx-6 transition-colors duration-300 hover:bg-green-500",
  },
  {
    onClick: () => play("scissors"),
    img: "./src/assets/ScissorsIcon.svg",
    alt: "Scissors",
    customClass:
      "bg-white rounded-full shadow-lg w-64 p-12 mx-6 transition-colors duration-300 hover:bg-yellow-500",
  },
];

const play = (c) => {
  choice.value = c;

  const choices = ["rock", "paper", "scissors"];
  const random = Math.floor(Math.random() * choices.length);
  computerChoice.value = choices[random];

  const outcome = outcomes[choice.value][computerChoice.value];

  if (outcome === "win") {
    wins.value++;
    verdict.value = "You win!";
  } else if (outcome === "loss") {
    losses.value++;
    verdict.value = "You Lose!";
  } else {
    draws.value++;
    verdict.value = "It's a draw";
  }

  SaveGame();
};

const winPercentage = computed(() => {
  const total = wins.value + draws.value + losses.value;
  return total ? (wins.value / total) * 100 : 0;
});

const SaveGame = () => {
  localStorage.setItem("wins", wins.value.toString());
  localStorage.setItem("draws", draws.value.toString());
  localStorage.setItem("losses", losses.value.toString());
};

const LoadGame = () => {
  wins.value = parseInt(localStorage.getItem("wins")) || 0;
  draws.value = parseInt(localStorage.getItem("draws")) || 0;
  losses.value = parseInt(localStorage.getItem("losses")) || 0;
};

const resetRound = () => {
  choice.value = "";
  computerChoice.value = "";
  verdict.value = "";
};

const startOver = () => {
  wins.value = 0;
  draws.value = 0;
  losses.value = 0;
  SaveGame();
};

onMounted(() => {
  LoadGame();

  window.addEventListener("keypress", (e) => {
    if (e.key === "r") {
      resetRound();
    } else if (e.key === "o") {
      startOver();
    } else if (e.key === "p") {
      props.isShowPauseScreen.value = true;
    }
  });
});

const emit = defineEmits(["show-pause"]);

const showPause = () => {
  emit("show-pause");
};

const props = defineProps({
  choice: String,
  computerChoice: String,
  verdict: String,
  isShowPauseScreen: Boolean,
});
</script>
