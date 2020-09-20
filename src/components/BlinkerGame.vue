<template>
  <div :class="{ 'the-game': true, 'the-game--easy': isEasy }">
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
    };
  },
  computed: {
    buttonStyle() {
      return `top: ${this.top}%; left: ${this.left}%;`
    },
    isEasy() {
      return this.difficulty === 'easy';
    }
  },
  methods: {
    clicked() {
      this.$emit('hit');
      this.top = randomInt(100);
      this.left = randomInt(100);
    }
  }
}
</script>

<style scoped>
.the-game {
  position: relative;
}

.the-game--easy {
  width: 30vw;
  height: 30vh;
  border: thin solid lightseagreen;
}

.the-game__button {
  position: absolute;
  white-space: nowrap;
}
</style>
