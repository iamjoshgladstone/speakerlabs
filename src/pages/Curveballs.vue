<template>
  <q-page>
    <div class="fit column wrap justify-center items-center content-cetner">
      <h5>Curveballs</h5>
      <!-- start timer -->
      <q-btn
        v-if="!isTimerStarted"
        class="fixed-center"
        color="primary"
        elevated
        rounded
        text-color="dark"
        label="Start"
        @click="startTimer"
      />

      <div class="text-h3 text-center q-mb-xl text-primary" v-if="isFinished">
        FINISHED
      </div>

      <div id="game-area" v-if="isTimerStarted">
        <!-- prompt section -->

        <div class="text-h4">{{ shownPrompt }}</div>

        <!-- word section -->
        <div class="absolute-center flex-center row fit">
          <div class="column">
            <div
              word-id="1"
              class="col"
              :class="[isActive1 ? 'active' : 'inactive']"
            >
              {{ wordList[wordArr[0]] }}
            </div>
            <div
              word-id="2"
              class="col"
              :class="[isActive2 ? 'active' : 'inactive']"
            >
              {{ wordList[wordArr[1]] }}
            </div>
            <div
              word-id="3"
              class="col"
              :class="[isActive3 ? 'active' : 'inactive']"
            >
              {{ wordList[wordArr[2]] }}
            </div>
          </div>
          <div class="column">
            <div
              word-id="4"
              class="col"
              :class="[isActive4 ? 'active' : 'inactive']"
            >
              {{ wordList[wordArr[3]] }}
            </div>
            <div
              word-id="5"
              class="col"
              :class="[isActive5 ? 'active' : 'inactive']"
            >
              {{ wordList[wordArr[4]] }}
            </div>
            <div
              word-id="6"
              class="col"
              :class="[isActive6 ? 'active' : 'inactive']"
            >
              {{ wordList[wordArr[5]] }}
            </div>
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
const time = 55;
const miliSecondTime = time * 1000;
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

watch(progress, (newCount, oldCount) => {
  // update words function
  if (newCount <= (1 / 6) * 6 - 0.02) {
    isActive1.value = true;
  }

  if (newCount <= (1 / 6) * 5 && newCount >= (1 / 6) * 4) {
    isActive2.value = true;
  }

  if (newCount <= (1 / 6) * 4 && newCount >= (1 / 6) * 3) {
    isActive3.value = true;
  }

  if (newCount <= (1 / 6) * 3 && newCount >= (1 / 6) * 2) {
    isActive4.value = true;
  }

  if (newCount <= (1 / 6) * 2 && newCount >= (1 / 6) * 1) {
    isActive5.value = true;
  }

  if (newCount <= (1 / 6) * 1) {
    isActive6.value = true;
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
