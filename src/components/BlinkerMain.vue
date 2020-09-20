<template>
  <div class="blinker-main">
    <h1>Welcome to Blinker</h1>
    <h3 v-show="!gameInProgress">Choose a difficulty</h3>
    <div class="blinker-main__button-row" v-show="!gameInProgress">
      <button @click="setDifficulty('easy')" class="blinker-main__button">easy</button>
      <button @click="setDifficulty('medium')" class="blinker-main__button">medium</button>
      <button @click="setDifficulty('hard')" class="blinker-main__button">hard</button>
    </div>
    <div v-show="!gameInProgress && counter">
      <h3>difficulty: {{ difficulty }}</h3>
      <h3>score: {{ counter }}</h3>
    </div>
    <div v-show="gameInProgress">
      difficulty: {{ difficulty }}<br/>
      score: {{ counter }} <br/>
      time left: {{ timeLeftSeconds }} seconds
    </div>
    <BlinkerGame v-if="gameInProgress" :difficulty="difficulty" @hit="buttonHit"></BlinkerGame>
  </div>
</template>

<script>
import BlinkerGame from "./BlinkerGame.vue";

const timerDuration = 3 * 1000;
export default {
  name: "BlinkerMain",
  components: {
    BlinkerGame
  },
  data() {
    return {
      difficulty: '',
      counter: 0,
      timeLeft: 30,
      started: 0,
      intervalHandle: 0
    }
  },
  computed: {
    gameInProgress() {
      return this.started !== 0;
    },
    timeLeftSeconds() {
      return this.timeLeft / 1000;
    }
  },
  methods: {
    setDifficulty(level) {
      this.difficulty = level;
      this.startTimer();
    },
    buttonHit() {
      this.counter++;
    },
    startTimer() {
      this.started = Date.now();
      this.counter = 0;
      this.intervalHandle = window.setInterval(this.timerHandler, 100);
    },
    stopTimer() {
      // console.log('stopTimer', this.intervalHandle, this.timeLeft);
      window.clearInterval(this.intervalHandle);
      this.timeLeft = timerDuration;
      this.started = 0;
    },
    timerHandler() {
      this.timeLeft = timerDuration - (Date.now() - this.started);
      // console.log('timerHandler', this.started, this.timeLeft, this.intervalHandle);
      if (this.timeLeft < 0) {
        this.stopTimer();
      }
    }
  }
}
</script>

<style scoped>
.blinker-main {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.blinker-main__button-row {
  display: flex;
  justify-content: center;
}

.blinker-main__button {
  margin: 0 15px;
}
</style>
