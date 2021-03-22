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