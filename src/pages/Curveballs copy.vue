<template>
  <q-page>
    <div class="fit column wrap justify-center items-center content-cetner">
      <h5>Curveballs</h5>
      <p>Pick your time to begin</p>
      <!-- start timer -->
      <div
        id="timer-select-area"
        class="column q-gutter-md"
        v-if="!isTimerStarted"
      >
        <q-btn
          class=""
          color="primary"
          elevated
          rounded
          text-color="dark"
          label="15 sec"
          @click="
            startTimer();
            setTime15();
          "
        />
        <q-btn
          class=""
          color="primary"
          elevated
          rounded
          text-color="dark"
          label="30 sec"
          @click="
            startTimer();
            setTime30();
          "
        />
        <q-btn
          class=""
          color="primary"
          elevated
          rounded
          text-color="dark"
          label="45 sec"
          @click="
            startTimer();
            setTime45();
          "
        />
        <q-btn
          class=""
          color="primary"
          elevated
          rounded
          text-color="dark"
          label="60 sec"
          @click="
            startTimer();
            setTime60();
          "
        />
      </div>

      <div class="text-h3 text-center q-mb-xl text-primary" v-if="isFinished">
        FINISHED
      </div>

      <div id="game-area" v-if="isTimerStarted">
        <!-- prompt section -->

        <div class="text-h4">{{ shownPrompt }}</div>

        <!-- word section -->
        <div class="absolute-center flex-center row fit">
          <div
            v-for="i in numOfWords"
            :key="i"
            :id="'word' + i"
            class="col inactive"
          >
            {{ wordList[wordArr[i]] }}
          </div>
        </div>
        <!-- Progress Bar -->
        <q-linear-progress
          rounded
          reverse
          instant-feedback
          size="20px"
          :value="progress"
          color="#fb3fb3"
          class="q-mt-sm absolute-bottom flex-center q-pl-md q-pr-md"
        />
      </div>
    </div>
  </q-page>
</template>

<script setup>
import { ref, watch } from "vue";

const isTimerStarted = ref(false);
const isFinished = ref(false);
const progress = ref(1);
const time = ref(55);
const numOfWords = ref(6);
const miliSecondTime = time.value * 1000;
const countdownTime = 0.01 * miliSecondTime;
const progressDecrement = 1 / (miliSecondTime / 100) / 2;
let timer;
const wordArr = ref([]);
const wordList = ref([
  "bug",
  "car",
  "stomach",
  "kitten",
  "science",
  "garden",
  "comb",
  "doorbell",
  "water",
]);

const promptList = ref([
  "The present moment",
  "Advice to your younger self",
  "How to be courageous",
  "What the future holds",
  "Your favourite memory",
]);
const shownPrompt = ref(null);

// setTime
const setTime15 = () => {
  time.value = 15;
};

// isActive Words
const isActive1 = ref(false);
const isActive2 = ref(false);
const isActive3 = ref(false);
const isActive4 = ref(false);
const isActive5 = ref(false);
const isActive6 = ref(false);

const startTimer = () => {
  generateWordArray();
  generatePrompt();
  isTimerStarted.value = true;
  isFinished.value = false;
  timer = setInterval(() => {
    progress.value = progress.value - progressDecrement;
  }, 50);
};

const generateWordArray = () => {
  while (wordArr.value.length < 6) {
    var r = Math.floor(Math.random() * wordList.value.length);
    if (wordArr.value.indexOf(r) === -1) wordArr.value.push(r);
  }
  console.log(wordArr.value);
};

const generatePrompt = () => {
  shownPrompt.value =
    promptList.value[Math.floor(Math.random() * promptList.value.length)];
};

const word1 = document.getElementById("word1");
const word2 = document.getElementById("word2");
const word3 = document.getElementById("word3");
const word4 = document.getElementById("word4");
const word5 = document.getElementById("word5");
const word6 = document.getElementById("word6");

watch(progress, (newCount, oldCount) => {
  // update words function
  if (newCount <= (1 / 6) * 6 - 0.02) {
    isActive1.value = true;
    word1.classList.add("active");
  }

  if (newCount <= (1 / 6) * 5 && newCount >= (1 / 6) * 4) {
    isActive2.value = true;
    word2.classList.add("active");
  }

  if (newCount <= (1 / 6) * 4 && newCount >= (1 / 6) * 3) {
    isActive3.value = true;
    word3.classList.add("active");
  }

  if (newCount <= (1 / 6) * 3 && newCount >= (1 / 6) * 2) {
    isActive4.value = true;
    word4.classList.add("active");
    word4.classList.remove("inactive");
  }

  if (newCount <= (1 / 6) * 2 && newCount >= (1 / 6) * 1) {
    isActive5.value = true;
    word5.classList.add("active");
    word5.classList.remove("inactive");
  }

  if (newCount <= (1 / 6) * 1) {
    isActive6.value = true;
    word6.classList.add("active");
    word6.classList.remove("inactive");
  }

  // // stop timer function
  if (oldCount <= 0 || newCount <= 0) {
    stopTimer();
  }
});

const stopTimer = () => {
  clearInterval(timer);
  isFinished.value = true;
  isTimerStarted.value = false;
  progress.value = 1;
  isActive1.value = false;
  isActive2.value = false;
  isActive3.value = false;
  isActive4.value = false;
  isActive5.value = false;
  isActive6.value = false;
  wordArr.value = [];
};
</script>

<style>
/* * {
  border: 1px solid red;
} */

.col {
  max-height: 200px;
  max-width: 300px;
  text-align: center;
  margin: 10px;
  font-size: 30px;
}

.inactive {
  opacity: 0;
}

.active {
  opacity: 1;
  transition: opacity 2s ease;
}
</style>
