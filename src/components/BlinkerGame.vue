<template>
  <div :class="{ 'the-game': true, 'the-game--easy': isEasy, 'the-game--medium': isMedium, 'the-game--hard': isHard }"
       :style="backgroundStyle">
    <button @click="clicked" class="the-game__button" :style="buttonStyle">Click me!</button>
  </div>
</template>

<script>
function randomInt(max) {
  return Math.floor(Math.random() * max);
}

export default {
  name: "BlinkerGame",
  props: {
    difficulty: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      top: randomInt(100),
      left: randomInt(100),
      hue: 0
    };
  },
  computed: {
    buttonStyle() {
      return `top: ${this.top}%; left: ${this.left}%;`
    },
    isEasy() {
      return this.difficulty === 'easy';
    },
    isMedium() {
      return this.difficulty === 'medium';
    },
    isHard() {
      return this.difficulty === 'hard';
    },
    backgroundStyle() {
      return `background-color: hsl(${this.hue}deg, 100%, 50%);`;
    }
  },
  methods: {
    clicked() {
      this.$emit('hit');
      this.top = randomInt(100);
      this.left = randomInt(100);
      this.hue = randomInt(360);
    }
  }
}
</script>

<style scoped>
.the-game {
  position: relative;
  transition: background-color 0.5s ease-in;
}

.the-game--easy {
  width: 30vw;
  height: 30vh;
  border: thin solid lightseagreen;
}

.the-game--medium {
  width: 50vw;
  height: 50vh;
  border: thin solid yellow;
}

.the-game--hard {
  width: 80vw;
  height: 80vh;
  border: thin solid orangered;
}

.the-game__button {
  position: absolute;
  white-space: nowrap;
}
</style>
