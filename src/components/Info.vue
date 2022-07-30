<!-- eslint-disable vue/multi-word-component-names -->
<!-- eslint-disable prettier/prettier -->
<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <p class="info">
    Letter count: <span id="score">{{score}}</span>,
    Miss count:<span id="miss">{{miss}}</span>,
    Time left: <span id="timer">{{timer}}</span>
  </p>
</template>

<script setup>
import { ref } from "vue";
import { toRefs } from "vue";

const timer = ref("0.00");
const startTime = ref();

const timeLimit = ref(5 * 1000);

const updateTimer = () => {
  const timeLeft = ref(startTime.value + timeLimit.value - Date.now());
  timer.value = (timeLeft.value / 1000).toFixed(2);

  const timeoutId = setTimeout(() => {
    updateTimer();
  }, 10);

  if (timeLeft.value < 0) {
    clearTimeout(timeoutId);
    timer.value = "0.00";
    emits("game-over");
    setTimeout(() => {
      showResult();
    }, 100);
  }
};

const showResult = () => {
  const accuracy =
    // eslint-disable-next-line prettier/prettier
    score.value + miss.value === 0 ? 0 : (score.value / (score.value + miss.value)) * 100;
  alert(
    // eslint-disable-next-line prettier/prettier
    `${score.value} letters, ${miss.value} misses, ${accuracy.toFixed(2)}% accuracy!`
  );
};

const gameStart = () => {
  startTime.value = Date.now();
  emits("game-init");
  updateTimer();
};

const props = defineProps({
  results: Object,
});

const { score, miss } = toRefs(props.results);

const emits = defineEmits(["game-init", "game-over"]);

defineExpose({
  gameStart,
});
</script>
