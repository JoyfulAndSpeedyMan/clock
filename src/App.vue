<template>
  <div>
      <!-- <router-link to="/clock">Go to clock</router-link> -->
  </div>
  <router-view>
    <div id="wa">
      <h1 id = "b">世界上最好的表</h1>
      <AnalogClock :size="80" unit="vmin"></AnalogClock>
    </div>
  </router-view>
</template>

<script>
import AnalogClock from '@/components/clock/AnalogClock.vue'
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
