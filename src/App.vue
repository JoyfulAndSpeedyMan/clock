<template>
  <div>
    <div id="wa">
      <h1 id = "b">我的表</h1>
      <AnalogClock :size="80" unit="vmin"></AnalogClock>
    </div>
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import AnalogClock from './components/clock/AnalogClock.vue'
export default {
  name: 'App',
  components: {
    AnalogClock
  },
  method: {

  },
  computed: {
  }
}
</script>

<style>
#wa {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

#b {
  text-align: center;
  font-size: 3em;
}
</style>
