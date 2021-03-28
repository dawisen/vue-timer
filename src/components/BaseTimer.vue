<!-- timer base -->
<template>
  <div class="base-timer">
    <svg
      class="base-timer__svg"
      viewBox="0 0 100 100"
      xmlns="http://www.w3.org/2000/svg"
    >
      <g class="base-timer__circle">
        <circle class="base-timer__path-elapsed" cx="50" cy="50" r="45" />
        <path
          :stroke-dasharray="circleDasharray"
          class="base-timer__path-remaining"
          d="
            M 50, 50
            m -45, 0
            a 45,45 0 1,0 90,0
            a 45,45 0 1,0 -90,0
          "
        ></path>
      </g>
    </svg>
    <span class="base-timer__label">
      <!-- remaining time label  -->
      {{ formattedTimeLeft }}
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
  &__label {
    position: absolute;
    // height and width need to be the same as the parent container's
    width: 300px;
    height: 300px;
    // align label to the top
    top: 0;
    //create flexbox to center the content
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 5rem;
  }
  &__path-remaining {
    /* same width as gray ring */
    stroke-width: 7px;
    /* Rounds the line endings to create a seamless circle */
    stroke-linecap: round;
    /* Makes sure the animation starts at the top of the circle */
    transform: rotate(90deg);
    transform-origin: center;
    /* One second aligns with the speed of the countdown timer */
    transition: 1s linear all;
    /* Allows the ring to change color when the color value updates */
    stroke: rgb(65, 184, 131); // green
    fill-rule: nonzero;
  }
  &__svg {
    /* Flips the svg and makes the animation to move left-to-right */
    transform: scaleX(-1);
  }
}
</style>

<script>
const FULL_DASH_ARRAY = 283;
const TIME_LIMIT = 20;
export default {
  data() {
    return {
      timePassed: 0,
      timerInterval: null,
    };
  },
  computed: {
    timeLeft() {
      return TIME_LIMIT - this.timePassed;
    },
    formattedTimeLeft() {
      const timeLeft = this.timeLeft;
      // The largest round integer less than or equal
      //   to the result of time divided being by 60.
      const minutes = Math.floor(timeLeft / 60);
      // Seconds are the remainder of the time divided
      //   by 60 (modulus operator)
      let seconds = timeLeft % 60;
      // If the value of seconds is less than 10,
      //   then display seconds with a leading zero
      if (seconds < 10) {
        seconds = `0${seconds}`;
      }
      // The output in MM:SS format
      return `${minutes}:${seconds}`;
    },

    // Update the dasharray value as time passes, starting with 283
    circleDasharray() {
      return `${(this.timeFraction * FULL_DASH_ARRAY).toFixed(0)} 283`;
    },
    timeFraction() {
      const rawTimeFraction = this.timeLeft / TIME_LIMIT;
      return rawTimeFraction - (1 / TIME_LIMIT) * (1 - rawTimeFraction);
    },
    colorCodes() {
      return {
        info: {
          color: "green"
        },
        warning: {
          color: "orange",
          threshold: this.warningThreshold
        },
        alert: {
          color: "red",
          threshold: this.alertThreshold
        }
      }
  },
  methods: {
    onTimesUp() {
      clearInterval(this.timerInterval);
    },
    startTimer() {
      this.timerInterval = setInterval(() => (this.timePassed += 1), 1000);
    },
  },
  watch: {
    timeLeft(newValue) {
      if (newValue === 0) {
        this.onTimesUp();
      }
    },
  },
  mounted() {
    this.startTimer();
    console.log("timer mounted!");
  },
   props: {
    alertThreshold: {
      type: Number,
      default: 5
     },
    warningThreshold: {
      type: Number,
      default: 10
    }
  }
};
</script>