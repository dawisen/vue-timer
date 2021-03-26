<template>
  <BaseTimer :time-left="timeLeft" />
</template>

<script>
import BaseTimer from './components/BaseTimer'
const TIME_LIMIT = 20;
export default {
  name: 'App',
  components: {
    BaseTimer
  },
  data() {
    return {
      timePassed: 0,
      timerInterval: null
    }
  },
  methods: {
      startTimer() {
      this.timerInterval = setInterval(() => (this.timePassed += 1), 1000);
    }
  },
  mounted() {
    this.startTimer();
    console.log('timer mounted!')
  },
  computed: {
    timeLeft() {
      return TIME_LIMIT - this.timePassed;
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  /* margin-top: 60px; */
}
</style>
