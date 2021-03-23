<!-- timer base -->
<template>
  <div class="base-timer">
    <svg
      class="base-timer__svg"
      viewBox="0 0 100 100"
      xmlns="http://www.w3.org/2000/svg"
    >
      <g class="base-timer__circle">
        <circle class="base-timer__path-elapsed" cx="50" cy="50" r="46.5" />
      </g>
    </svg>
    <span class="base-timer__label">
      <!-- remaining time label  -->
      <p>time</p>
    </span>
  </div>
</template>

<style lang="scss" scoped>
/* set the container height and width */
#timer-container {
  justify-content: center;
}

.base-timer {
  // always use display block/margin auto to center SVG images
  display: block;
  margin: auto;
  width: 300px;
  height: 300px;

  /* remove svg styling that would hide the time label */
  &__circle {
    fill: none;
    stroke: none;
  }
  /* SVG path around circle displaying timer progress */
  &__path-elapsed {
    stroke-width: 7px;
    stroke: grey;
  }
}
</style>

<script>
  export default {
    computed: {
      formattedTimeLeft() {
      const timeLeft = this.timeLeft
      // The largest round integer less than or equal 
         to the result of time divided being by 60.
      const minutes = Math.floor(timeLeft / 60)
      // Seconds are the remainder of the time divided
         by 60 (modulus operator)
      let seconds = timeLeft % 60
      // If the value of seconds is less than 10,
         then display seconds with a leading zero
      if (seconds < 10) {
        seconds = `0${seconds}`
      }
      // The output in MM:SS format
      return `${minutes}:${seconds}`
      }
    }
  }
</script>