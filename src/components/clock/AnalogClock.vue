<template>
  <div :style="wapper">
    <div class="clock" :style="clockStyle">
      <div class="knob center"></div>
      <div class="box seconds" :style="secondRotateStyle">
        <div class="arm second" :style="secondStyle"></div>
      </div>
      <div class="box minutes" :style="minuteRotateStyle">
        <div class="arm minute" :style="minuteStyle"></div>
      </div>
      <div class="box hours" :style="hourRotateStyle">
        <div class="arm hour" :style="hourStyle"></div>
      </div>

      <div class="minute-lines">
        <div v-for="i in 60" :key="i" :a="i" class="minute-line"
          :style="`transform: translate(0%, -50%) rotate(${calibDeg(i)}deg)`">

          <div class="minute-line-box">

            <div class="clock-scale-text" :style="clockScaleTextStyle(i)" v-if="showCalibDigit(i)">
              {{ i != 1 ? (i - 1) / 5 : 12 }}
            </div>

            <div class="clock-scale" :style="scaleStyle(i)"></div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import('@/assets/css/clock_analog.css')
export default {
  name: 'AnalogClock',
  props: {
    size: {
      type: Number,
      default: 500
    },
    unit: {
      type: String,
      default: 'px'
    }
  },
  mounted() {
    let date = new Date();
    let seconds = date.getSeconds() * 6;
    let minutes = date.getMinutes() * 6 + seconds / 60;
    let hours = (180 + date.getHours() % 12 * 30 + minutes / 15);

    this.secondRotateStyle = {
      transform: `rotate(${180 + seconds}deg)`
    };
    this.minuteRotateStyle = {
      transform: `rotate(${180 + minutes}deg)`
    };
    this.hourRotateStyle = {
      transform: `rotate(${hours}deg)`
    };
  },
  data() {
    return {
      secondRotateStyle: {},
      minuteRotateStyle: {},
      hourRotateStyle: {}
    }
  },
  methods: {
    onClick() {
      console.log(this.secondRotateStyle)
    },
    calibDeg(i) {
      return (i - 1) * 6 - 90;
    },
    showCalibDigit(i) {
      return (i - 1) % 5 == 0;
    },
    scaleStyle(i) {
      let style = this.showCalibDigit(i) ? this.bigScaleStyle : this.minScaleStyle;
      return style;
    },
    clockScaleTextStyle(i) {
      return {
        transform: `rotate(${-this.calibDeg(i)}deg)`,
        fontSize: this.clockSize / 15 + this.unit
      }
    }
  },
  computed: {
    clockBorderWidth() {
      return this.clockBorderSize + this.unit;
    },
    clockBorderSize() {
      return this.size * 0.02;
    },
    clockSize() {
      return this.size - this.clockBorderSize;
    },

    secondWidth() {
      return this.clockSize * 0.005;
    },
    secondHeight() {
      return this.clockSize * 0.45;
    },

    minuteWidth() {
      return this.secondWidth * 1.2;
    },
    minuteHeight() {
      return this.secondHeight * 0.7;
    },

    hourWidth() {
      return this.secondWidth * 1.4;
    },
    hourHeight() {
      return this.secondHeight * 0.7 * 0.7;
    },
    minScaleWidth() {
      return this.minScaleHeigth * 3;
    },
    minScaleHeigth() {
      return (this.clockSize / 2) * 0.015
    },
    bigScaleWidth() {
      return this.bigScaleHeigth * 5;
    },
    bigScaleHeigth() {
      return (this.clockSize / 2) * 0.025
    },

    secondStyle() {
      return {
        width: this.secondWidth + this.unit,
        height: this.secondHeight + this.unit,
        borderRadius: this.secondWidth + this.unit
      }
    },
    minuteStyle() {
      return {
        width: this.minuteWidth + this.unit,
        height: this.minuteHeight + this.unit,
        borderRadius: this.minuteWidth * 1.2 + this.unit
      }
    },
    hourStyle() {
      return {
        width: this.hourWidth + this.unit,
        height: this.hourHeight + this.unit,
        borderRadius: this.hourWidth + this.unit
      }
    },
    minScaleStyle() {
      return {
        width: this.minScaleWidth + this.unit,
        height: this.minScaleHeigth + this.unit,
      }
    },
    bigScaleStyle() {
      return {
        width: this.bigScaleWidth + this.unit,
        height: this.bigScaleHeigth + this.unit,
      }
    },
    clockStyle() {
      return {
        width: this.clockSize + this.unit,
        height: this.clockSize + this.unit,
        borderRadius: this.clockSize + this.unit,
        boxShadow: `0px 0px 0px ${this.clockBorderWidth} #222`
      }
    },
    wapper() {
      return {
        height: this.size + this.unit,
        width: this.size + this.unit,
        padding: `${this.clockBorderWidth} `

      }
    }
  }
}

</script>