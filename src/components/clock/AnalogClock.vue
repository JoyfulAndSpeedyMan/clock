<template>
  <div :style="wapper">
    <div class="clock" :style="clockStyle">
      <div class="knob center" :style="knobStyle"></div>
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
        <div v-for="i in 60" :key="i" :a="i" class="minute-line" :style="minuteLineStyle(i)">
          <div class="minute-line-outer-box">
            <div class="minute-line-box">
              <div class="clock-scale" :style="scaleStyle(i)"></div>

              <div class="clock-scale-text" :style="clockScaleTextStyle(i)" v-if="showCalibDigit(i)">
                {{ i != 1 ? (i - 1) / 5 : 12 }}
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// import('@/assets/css/clock_analog.css')
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
      let style = this.showCalibDigit(i) ? {
        width: this.bigScaleWidth + this.unit,
        height: this.bigScaleHeigth + this.unit,
        marginTop: this.scaleHeigth / 2 - this.bigScaleHeigth / 2 + this.unit
      } : {
        width: this.minScaleWidth + this.unit,
        height: this.minScaleHeigth + this.unit,
        marginTop: this.scaleHeigth / 2 - this.minScaleHeigth / 2 + this.unit
      };
      return style;
    },
    clockScaleTextStyle(i) {
      let fontSize = this.clockSize / 15;
      return {
        transform: `rotate(${-this.calibDeg(i)}deg)`,
        fontSize: fontSize  + this.unit,
        lineHeight: this.scaleHeigth + this.unit,
        marginRight: fontSize / 5 + this.unit
      }
    },
    minuteLineStyle(i) {
      return {
        transform: `translate(0%, -50%) rotate(${this.calibDeg(i)}deg)`,
        height: this.scaleHeigth + this.unit
      };
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
    knobWidth() {
      return this.clockSize * 0.06;
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
    scaleHeigth() {
      return this.clockSize * 0.1;
    },

    knobStyle() {
      return {
        width: this.knobWidth + this.unit,
        height: this.knobWidth + this.unit,
        borderRadius: this.knobWidth + this.unit
      }
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

<style scoped>
.clock {
  width: 200px;
  height: 200px;
  box-shadow: 0px 0px 0px 10px #222;
  border-radius: 200px;
  position: relative;
  top: 0;
  left: 0;
}

.center {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.knob {
  background: #111;
  z-index: 4;
}

.arm {
  position: absolute;
  top: 50%;
  left: 50%;
  transform-origin: top left 0;
  z-index: 2;
  width: 0;
  animation: rotateArms linear 60s infinite;
  -webkit-animation: rotateArms linear 60s infinite;
  -moz-animation: rotateArms linear 60s infinite;
}

.minute.arm {
  background-color: #444;
  box-shadow: 0 0 0 2px #444;
  animation-duration: 3600s;
  -webkit-animation-duration: 3600s;
  -moz-animation-duration: 3600s;
}

.hour.arm {
  background-color: #444;
  box-shadow: 0 0 0 3.33333px #444;
  animation-duration: 43200s;
  -webkit-animation-duration: 43200s;
  -moz-animation-duration: 43200s;
}

.second.arm {
  /* width: 10px; */
  box-shadow: 0 0 0 1.33333px #a00;
  background-color: #a00;
}

.box {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}

.minute-lines {
  width: 100%;
  height: 100%;
  z-index: -2;
}

.minute-line {
  position: absolute;
  top: 50%;
  left: 50%;
  transform-origin: 0 50% 0;
  /* background-color: rgb(186, 189, 39); */
  width: 50%;
  /* height: 10%; */
  /* z-index: -1; */

}

.minute-line-outer-box {
  width: 100%;
  height: 100%;
  position: absolute;
  right: 2%;
  /* background-color: rgb(108, 109, 30); */
}

.minute-line-box {
  position: absolute;
  height: 100%;
  right: 0;
  /* background-color: rgb(186, 189, 39); */
}

.minute-line-box .clock-scale-text {
  display: block;
  float: right
}

.minute-line-box .clock-scale {
  display: block;
  float: right;
  height: 20%;
  width: 50px;
  background-color: #000;
  margin-top: 50%;
}

@keyframes rotateArms {
  from {
    transform: rotate(0) translate(-50%, 0);
  }

  to {
    transform: rotate(360deg) translate(-50%, 0);
  }
}

@-webkit-keyframes rotateArms {
  from {
    transform: rotate(0) translate(-50%, 0);

  }

  to {
    transform: rotate(360deg) translate(-50%, 0);
  }
}

@-moz-keyframes rotateArms {
  from {
    transform: rotate(0) translate(-50%, 0);
  }

  to {
    transform: rotate(360deg) translate(-50%, 0);
  }
}
</style>