<template>
  <Layout>
    <main class="container mx-auto p-6 flex-1">
      <div v-if="!showPauseScreen">
        <div
          v-if="choice === null"
          class="flex items-center justify-center -mx-6"
        >
          <button
            @click="play('rock')"
            class="bg-white rounded-full shadow-lg w-64 p-12 mx-6 transition-colors duration-300 hover:bg-pink-500"
          >
            <img src="./assets/RockIcon.svg" alt="Rock" class="w-full" />
          </button>

          <button
            @click="play('paper')"
            class="bg-white rounded-full shadow-lg w-64 p-12 mx-6 transition-colors duration-300 hover:bg-green-500"
          >
            <img src="./assets/PaperIcon.svg" alt="Paper" />
          </button>

          <button
            @click="play('scissors')"
            class="bg-white rounded-full shadow-lg w-64 p-12 mx-6 transition-colors duration-300 hover:bg-yellow-500"
          >
            <img src="./assets/ScissorsIcon.svg" alt="Scissors" />
          </button>
        </div>

        <div v-else>
          <Message
            :preText="`You picked`"
            :mainText="choice"
            :commonStyles="`text-3xl mb-4`"
            :mainStyles="`text-pink-500`"
          />
          <Message
            :preText="`The computer picked`"
            :mainText="computerChoice"
            :afterText="`pppp`"
            :commonStyles="`text-3xl mb-4`"
            :mainStyles="`text-green-500`"
          />
          <Message :mainText="verdict" :mainStyles="`text-6xl mb-12`" />

          <ReusableButton
            label="Reset"
            :onClick="ResetRound"
            customClass="bg-pink-500 text-lg py-2 px-4"
          />
        </div>

        <div class="mt-12 text-3xl mb-4">
          {{ wins }} : {{ draws }} : {{ losses }}
        </div>

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

      <div v-if="showPauseScreen" class="mt-12">
        <Message
          :mainText="`This is the Pause Screen!`"
          :mainStyles="`text-3xl`"
        />
        <ReusableButton
          label="Play Again"
          :onClick="removePause"
          customClass="bg-green-500 text-lg py-2 px-4 mt-4"
        />
      </div>
    </main>
  </Layout>
</template>

<script setup>
import { ref, computed, onMounted } from "vue";
import ReusableButton from "./components/main/ReusableButton.vue";
import Layout from "./components/layout/Layout.vue";
import Message from "./components/main/Message.vue";

const wins = ref(0);
const draws = ref(0);
const losses = ref(0);

const choice = ref(null);
const computerChoice = ref(null);
const verdict = ref(null);

const outcomes = {
  rock: {
    rock: "draw",
    paper: "loss",
    scissors: "win",
  },
  paper: {
    rock: "win",
    paper: "draw",
    scissors: "loss",
  },
  scissors: {
    rock: "loss",
    paper: "win",
    scissors: "draw",
  },
};

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
  localStorage.setItem("wins", wins.value);
  localStorage.setItem("draws", draws.value);
  localStorage.setItem("losses", losses.value);
};

const LoadGame = () => {
  wins.value = parseInt(localStorage.getItem("wins")) || 0;
  draws.value = parseInt(localStorage.getItem("draws")) || 0;
  losses.value = parseInt(localStorage.getItem("losses")) || 0;
};

const ResetRound = () => {
  choice.value = null;
  computerChoice.value = null;
  verdict.value = null;
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
      ResetRound();
    } else if (e.key === "o") {
      startOver();
    } else if (e.key === "p") {
      showPauseScreen.value = true;
    }
  });
});

const showPauseScreen = ref(false);
const removePause = () => {
  showPauseScreen.value = false;
};
const showPause = () => {
  showPauseScreen.value = true;
};
</script>
