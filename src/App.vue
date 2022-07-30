<template>
  <Target
    ref="targetComp"
    @game-start="gameStart"
    @game-reset="getScore"
    @send-results="getScore"
  />
  <Info
    ref="infoRef"
    @game-init="gameInit"
    @game-over="gameOver"
    :results="results"
  />
</template>

<script setup>
import "./assets/base.css";

import { ref } from "vue";

import Target from "./components/Target.vue";
import Info from "./components/Info.vue";

const targetComp = ref();
const infoRef = ref();

const results = ref({
  score: 0,
  miss: 0,
});

const gameInit = () => {
  targetComp.value.gameInit();
};

const getScore = (score, miss) => {
  results.value.score = score;
  results.value.miss = miss;
};

const gameStart = () => {
  infoRef.value.gameStart();
};

const gameOver = () => {
  targetComp.value.gameOver();
};
</script>
