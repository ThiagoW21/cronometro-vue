<script setup>
import { ref } from "vue";
import Time from "./components/Time.vue";
import Timer from "./components/Timer.vue";

const timer = ref(false);
const resetTimer = ref(false);
const timers = ref([]);
const textContent = ref("Start");
const save = ref(false);

function handleTimer() {
  if (resetTimer.value) {
    resetTimer.value = false;
  }
  timer.value = !timer.value;
  textContent.value = timer.value ? "Stop" : "Start";
}

function setTime(event) {
  timers.value.push(event);
  save.value = false;
}

function saveTime() {
  save.value = true;
}

function resetTime() {
  resetTimer.value = true;
  timer.value = false;
  textContent.value = "Start";
}
</script>

<template>
  <header>
    <img
      alt="Vue logo"
      class="logo"
      src="@/assets/logo.svg"
      width="125"
      height="125"
    />
    <Timer
      :resetTimer="resetTimer"
      :start="timer"
      @set-time="setTime"
      :saveTime="save"
    />
    <div class="wrapper">
      <button
        class="btn-start"
        :class="[timer && 'btn-stop']"
        type="button"
        @click="handleTimer"
      >
        {{ textContent }}
      </button>
      <button type="button" @click="saveTime">Marcar tempo</button>
      <button class="btn-reset" type="button" @click="resetTime">
        Reiniciar
      </button>
    </div>
  </header>
  <div class="time-container">
    <div v-for="(time, index) in timers" :key="index">
      <Time :time="time" />
    </div>
  </div>
</template>

<style>
@import "@/assets/base.css";

#app {
  max-width: 1280px;
  margin: 0 auto;
  padding: 2rem;
  font-weight: normal;
  display: flex;
}

.time-container {
  display: flex;
  flex-direction: column;
  max-height: 300px;
  width: 100px;
  flex-wrap: wrap;
}

.wrapper {
  display: flex;
  width: 500px;
  justify-content: center;
}

button {
  width: auto;
  height: 30px;
  font-weight: 600;
  border: 2px solid #41b883;
  background-color: #181818;
  color: #41b883;
  border-radius: 5px;
  margin: 5px;
  transition: 0.1s;
  white-space: nowrap;
}

button:hover {
  background-color: #41b88362;
}

.btn-stop:hover {
  background-color: #b8414162;
}

header {
  line-height: 1.5;
  max-height: 100vh;
}

.logo {
  margin: 0 auto 2rem;
}

a,
.green {
  text-decoration: none;
  color: hsla(160, 100%, 37%, 1);
  transition: 0.4s;
}

@media (hover: hover) {
  a:hover {
    background-color: hsla(160, 100%, 37%, 0.2);
  }
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}

body {
  display: flex;
  width: 100%;
  height: 100%;
}

@media (min-width: 1024px) {
  body {
    display: flex;
    place-items: center;
  }

  header {
    width: 500px;
    display: flex;
    align-items: center;
    flex-direction: column;
  }
}
</style>
