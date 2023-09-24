<template>
  <div class="progress">
    <svg class="progress-bar" xmlns="http://www.w3.org/2000/svg" width="400" height="400" viewPort="0 0 200 200">

      <!-- градиенты -->
      <defs>
        <linearGradient id="progress" x1="0" x2="0" y1="0" y2="1" spreadMethod="pad">
          <stop offset="0%" stop-color="#ff8000"/>
          <stop offset="100%" stop-color="#ffb300"/>
        </linearGradient>

        <linearGradient id="bg" x1="0" x2="0" y1="0" y2="1" spreadMethod="pad">
          <stop offset="0%" stop-color="#303030"/>
          <stop offset="100%" stop-color="#575757"/>
        </linearGradient>

        <linearGradient id="success" x1="0" x2="0" y1="0" y2="1" spreadMethod="pad">
          <stop offset="0%" stop-color="#00d70e"/>
          <stop offset="100%" stop-color="#039300"/>
        </linearGradient>
      </defs>
      <!-- круг бг-->
      <circle cx="200" cy="200" r="180" stroke="url(#bg)"
              class="progress-bar__background"/>
      <!-- круг прогресс -->
      <circle cx="200" cy="200" r="180" stroke-dasharray="565" stroke-dashoffset="0"
              :stroke="value < 80 ? 'url(#progress)' : 'url(#success)'"
              id="progressBar"
              class="progress-bar__progress"/>

      <text x="175" y="28">
        <tspan style="font-weight: bold; font-size: 24px; fill: #fff; opacity: .5">
          {{ (value < targetValue) ? targetValue : '' }}
        </tspan>
      </text>
      <text x="130" y="-365" style="transform: rotate(90deg); opacity: .5">
        <tspan style="font-weight: bold; font-size: 24px; fill: #fff">{{ (value < maxValue) ? maxValue : '' }}</tspan>
      </text>
    </svg>

    <div v-if="value < targetValue" class="progress__text">
      <h3>Еще продать на завтра</h3>
      <div class="progress__info"><h2 class="progress__value">{{ targetValue - value }}</h2><span>&nbsp;ч</span></div>
    </div>
    <div v-else class="progress__text">
      <h2 class="progress__value progress__value_success"> {{ targetValue }} </h2>
    </div>

    <!-- штука для точки внутри прогресса -->
    <div class="progress__outer">
      <div class="progress__inner">{{ value }}</div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'CircleProgress',
  props: {
    maxValue: Number,
    value: Number,
    targetValue: Number
  },
  computed: {
    getStrokeDashOffsetValue() {
      return Math.round(1130 - (this.value / this.maxValue * 1130));
    },
  },
  methods: {
    addCss() {
      const progressBar = document.getElementById('progressBar');
      const outer = document.getElementsByClassName('progress__outer')[0];
      const inner = document.getElementsByClassName('progress__inner')[0];
      console.log(this.getStrokeDashOffsetValue)
      progressBar.style.strokeDashoffset = this.getStrokeDashOffsetValue
      outer.style.transform = 'rotate(' + (this.value / this.maxValue * 3.6 * 100) + 'deg)'
      inner.style.transform = 'rotate(-' + (this.value / this.maxValue * 3.6 * 100) + 'deg)'
    }
  },
  mounted() {
    setTimeout(() => {
      this.addCss()
    }, 100)
  },
  watch: {
    value: function () {
      this.addCss()
    }
  }
}
</script>

<style lang="scss">
.progress {
  display: flex;
  position: relative;

  &__text {
    position: absolute;
    width: 400px;
    height: 400px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }

  &__info {
    display: flex;
    align-items: baseline;
  }

  &__value {
    font-size: 30px;
    color: orange;

    &_success {
      color: green;
    }
  }

  &__outer {
    --outer-size: 400px;
    position: absolute;
    width: var(--outer-size);
    height: var(--outer-size);
    border-radius: 50%;
    transform: rotate(0deg);
    transition: all 1s ease-in-out;
  }

  &__inner {
    --inner-size: 40px;
    display: flex;
    align-items: center;
    justify-content: center;
    position: absolute;
    width: var(--inner-size);
    height: var(--inner-size);
    transform: rotate(0deg);
    transition: all 1s ease-in-out;
    left: calc(var(--outer-size) / 2 - var(--inner-size) / 2);
    background: #545353;
    border-radius: 50%;
    color: #ff8000;
    font-size: 20px;
    font-weight: bold;
    line-height: 0;
  }
}

$progress-bar-stroke-width: 46;
$progress-bar-size: 400px;

.progress-bar {
  height: $progress-bar-size;
  transform: rotate(-90deg);
  width: $progress-bar-size;
  overflow: visible;

  &__background {
    fill: none;
    stroke-width: $progress-bar-stroke-width;
  }

  &__progress {
    position: relative;
    fill: none;
    stroke-dasharray: 1130 1130; // 2 * pi * R
    stroke-dashoffset: 1130; // 2 * pi * R
    stroke-linecap: round;
    stroke-width: $progress-bar-stroke-width;
    transition: all 1s ease-in-out;
  }
}
</style>
