<script setup>
import { computed, onBeforeUpdate, onUpdated, ref } from "vue";

const props = defineProps({
  start: {
    type: Boolean,
    required: true,
  },
  resetTimer: {
    type: Boolean,
    required: true,
  },
  saveTime: {
    type: Boolean,
    required: true,
  },
});

const emit = defineEmits({
  "save-time": {
    type: Function,
    required: true,
  },
});

const minutes = ref(0);
const seconds = ref(0);
const miliseconds = ref(0);
const timer = ref(0);

const textTimer = computed(() => {
  return `
  ${("00" + minutes.value).slice(-2)}:${("00" + seconds.value).slice(-2)}:${(
    "00" + miliseconds.value
  ).slice(-2)}`;
});

function continueTimer() {
  timer.value = setTimeout(() => {
    if (miliseconds.value === 98) {
      seconds.value += 1;
      miliseconds.value = 0;
    }
    if (seconds.value === 59) {
      seconds.value = 0;
      minutes.value += 1;
    }
    miliseconds.value += 1;
  }, 9.5);
}

function reset() {
  timer.value = 0;
  minutes.value = 0;
  seconds.value = 0;
  miliseconds.value = 0;
}

onUpdated(() => {
  if (props.resetTimer) {
    reset();
  }

  if (props.saveTime) {
    emit("set-time", textTimer.value);
  }

  if (props.start) {
    continueTimer();
  }
});

onBeforeUpdate(() => {
  clearTimeout(timer.value);
});
</script>

<template>
  <div class="greetings">
    <h3>{{ textTimer }}</h3>
  </div>
</template>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

.greetings {
  margin: 20px;
}

@media (min-width: 1024px) {
  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style>
