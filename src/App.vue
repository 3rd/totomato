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
      timerElapsedSeconds: 0, // Elapsed seconds
      timerText: '00:00', // Formatted timer text
      timerInterval: null, // Holds interval object,
      timerTimeout: null, // Holds timeout object
      workSessionMinutes: 0.2, // Config
      breakSessionMinutes: 0.1 // Config
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
    },
    timerStateText: function () {
      if (this.timerState === 0) {
        return 'Idle'
      }
      if (this.timerState === 1) {
        return 'Working'
      }
      if (this.timerState === 2) {
        return 'On Break'
      }
    },
    timerText: function () {
      if (this.isIdle) {
        return 'Work'
      }
      if (this.isWorking) {
        var secondsLeftTotal = this.workSessionMinutes * 60 - this.timerElapsedSeconds
        var minutesLeft = Math.floor(secondsLeftTotal / 60)
        var secondsLeft = secondsLeftTotal % 60
        return minutesLeft + ':' + secondsLeft
      }
    }
  },
  components: {
    appHeader,
    appTimer
  },
  methods: {
    primaryAction: function () {
      var self = this
      if (this.isIdle) {
        this.timerState = 1
        this.timerElapsedSeconds = 0
        this.timerInterval = setInterval(function () {
          self.timerElapsedSeconds++
        }, 1000)
        this.timerTimeout = setTimeout(function () {
          self.timerState = 0
          clearInterval(self.timerInterval)
          clearTimeout(self.timerTimeout)
        }, 1000 * 60 * self.workSessionMinutes - 1000)
      } else {
        this.timerState = 0
        clearInterval(this.timerInterval)
        clearTimeout(this.timerTimeout)
      }
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
