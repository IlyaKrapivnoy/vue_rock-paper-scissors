<template>
  <Layout>
    <main class="container mx-auto p-6 flex-1">
      <div v-if="!showPauseScreen">
        <MainScreen
          :choice="choice"
          :computerChoice="computerChoice"
          :verdict="verdict"
          @showPause="showPauseScreen = true"
        />
      </div>

      <div v-if="showPauseScreen" class="mt-12">
        <PauseScreen @onRemovePause="removePause" />
      </div>
    </main>
  </Layout>
</template>

<script setup>
import { ref } from "vue";
import Layout from "./components/layout/Layout.vue";
import PauseScreen from "./components/screens/PauseScreen.vue";
import MainScreen from "./components/screens/MainScreen.vue";

const choice = ref("");
const computerChoice = ref("");
const verdict = ref("");

const showPauseScreen = ref(false);
const removePause = () => {
  showPauseScreen.value = false;
};
</script>
