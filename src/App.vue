<template>
  <Layout>
    <main class="container mx-auto p-6 flex-1">
      <div v-if="!isShowPauseScreen">
        <MainScreen
          :choice="choice"
          :computerChoice="computerChoice"
          :verdict="verdict"
          @showPause="showPauseScreen"
        />
      </div>

      <div v-if="isShowPauseScreen" class="mt-12">
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

const isShowPauseScreen = ref(false);

const showPauseScreen = () => {
  isShowPauseScreen.value = true;
};

const removePause = () => {
  isShowPauseScreen.value = false;
};
</script>
