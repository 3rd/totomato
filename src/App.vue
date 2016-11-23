<template>
  <div id="app">
    <app-header
              :timer-state-text="timerStateText"
              class="clearfix">
    </app-header>
    <app-timer
              v-on:action="primaryAction"
              :timer-text="timerText">
    </app-timer>
  </div>
</template>

<script>
import appHeader from './components/app-header'
import appTimer from './components/app-timer'

export default {
  name: 'app',
  data: function () {
    return {
      timerState: 0, /*
                        Timer states:
                              0 - idle
                              1 - working
                              2 - on break
                     */
      timerStateText: 'Idle',
      timerSeconds: 0, // Elapsed seconds
      timerText: '00:00', // Formatted timer text
      timerInterval: null // Holds interval object
    }
  },
  computed: {
    isIdle: function () {
      return this.timerState === 0
    },
    isWorking: function () {
      return this.timerState === 1
    },
    isOnBreak: function () {
      return this.timerState === 2
    }
  },
  components: {
    appHeader,
    appTimer
  },
  methods: {
    primaryAction: function () {
      //
    }
  }
}
</script>

<style>
body {
  background: #f6f6f6;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.clearfix:after {
     visibility: hidden;
     display: block;
     font-size: 0;
     content: " ";
     clear: both;
     height: 0;
     }
.clearfix { display: inline-block; }
* html .clearfix { height: 1%; }
.clearfix { display: block; }
</style>
