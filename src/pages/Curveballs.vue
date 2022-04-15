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
          @click="startTimer(15, 2)"
        />
        <q-btn
          class=""
          color="primary"
          elevated
          rounded
          text-color="dark"
          label="30 sec"
          @click="startTimer(30, 4)"
        />
        <q-btn
          class=""
          color="primary"
          elevated
          rounded
          text-color="dark"
          label="45 sec"
          @click="startTimer(45, 6)"
        />
        <q-btn
          class=""
          color="primary"
          elevated
          rounded
          text-color="dark"
          label="60 sec"
          @click="startTimer(60, 8)"
        />
      </div>

      <div class="text-h3 text-center q-mb-xl text-primary" v-if="isFinished">
        FINISHED
      </div>

      <div id="game-area" v-if="isTimerStarted">
        <!-- prompt section -->

        <div class="text-h4">{{ shownPrompt }}</div>

        <!-- word section -->
        <div class="absolute-center flex-center row" id="words">
          <div
            v-for="i in numOfWords"
            :key="i"
            :id="'word' + i"
            class="col-6 inactive"
          >
            {{ wordList[wordArr[i - 1]] }}
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
let i;

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

const startTimer = (setTime, numWords) => {
  time.value = setTime;
  numOfWords.value = numWords;
  const miliSecondTime = time.value * 1000;
  const countdownTime = 0.01 * miliSecondTime;
  const progressDecrement = 1 / (miliSecondTime / 100) / 2;
  generateWordArray();
  generatePrompt();
  isTimerStarted.value = true;
  isFinished.value = false;
  i = numWords - 1;
  timer = setInterval(() => {
    progress.value = progress.value - progressDecrement;
  }, 50);
};

const generateWordArray = () => {
  while (wordArr.value.length < numOfWords.value) {
    var r = Math.floor(Math.random() * wordList.value.length);
    if (wordArr.value.indexOf(r) === -1) wordArr.value.push(r);
  }
  console.log(wordArr.value);
};

const generatePrompt = () => {
  shownPrompt.value =
    promptList.value[Math.floor(Math.random() * promptList.value.length)];
};

// quadrants
let n = 0;

watch(progress, (newCount, oldCount) => {
  const quadrants = 1 / numOfWords.value;
  console.log(i, n, quadrants);
  const elements = document.getElementById("words").children;
  // elements.item(n).classList.add("active"); // add this in if you want words to appear immediately

  /*  /// // stop timer functions*/

  if (oldCount <= 0 || newCount <= 0) {
    stopTimer();
  }

  if (
    newCount <= quadrants * i &&
    newCount >= quadrants * (i - 1) &&
    n <= numOfWords.value - 1
  ) {
    elements.item(n).classList.add("active");
    n++;
    i--;
  }
});

const stopTimer = () => {
  clearInterval(timer);
  isFinished.value = true;
  isTimerStarted.value = false;
  progress.value = 1;
  wordArr.value = [];
  n = 0;
  i = numOfWords.value - 1;
};
</script>

<style>
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
