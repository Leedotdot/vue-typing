<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <p id="target">{{ word }}</p>
</template>

<script setup>
import { ref } from "vue";

const isPlaying = ref(false);

const words = ["apple", "sky", "blue", "middle", "set"];
const word = ref("click to start");

const score = ref(0);
const miss = ref(0);
const loc = ref(0);

const windowClick = () => {
  window.addEventListener("click", () => {
    if (isPlaying.value === true) {
      return;
    }
    isPlaying.value = true;
    updateWord();
    emits("game-start");
  });
};
windowClick();

const gameInit = () => {
  score.value = 0;
  miss.value = 0;
  emits("game-reset", score.value, miss.value);
};

const updateWord = () => {
  word.value = words[Math.floor(Math.random() * words.length)];
};

window.addEventListener("keydown", (e) => {
  if (isPlaying.value === false) {
    return;
  }
  if (e.key === word.value[loc.value]) {
    loc.value++;
    score.value++;
    if (word.value.length === loc.value) {
      loc.value = 0;
      updateWord();
    }
    let placeholder = "";
    for (let i = 0; i < loc.value; i++) {
      placeholder += "_";
    }
    word.value = placeholder + word.value.substring(loc.value);
  } else {
    miss.value++;
  }
  emits("send-results", score.value, miss.value);
});

const gameOver = () => {
  word.value = "click to start";
  loc.value = 0;
  isPlaying.value = false;
};

const emits = defineEmits(["game-reset", "send-results", "game-start"]);

defineExpose({
  gameInit,
  gameOver,
});
</script>
