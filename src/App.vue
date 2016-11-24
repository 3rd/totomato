<template>
  <div id="app">
    <app-header
              :timer-state-text="timerStateText"
              v-on:settings-action="settingsAction"
              class="clearfix">
    </app-header>
    <app-timer
              v-on:primary-action="primaryAction"
              :timer-text="timerText">
    </app-timer>
    <app-settings
              v-if="settingsVisible"
              :work-session-minutes="workSessionMinutes"
              :break-session-minutes="breakSessionMinutes"
              v-on:settings-cancel="settingsCancel"
              v-on:settings-save="settingsSave">
    </app-settings>
    <app-history
              :completed-sessions-count="completedSessionsCount">
    </app-history>
  </div>
</template>

<script>
import appHeader from './components/app-header'
import appTimer from './components/app-timer'
import appSettings from './components/app-settings'
import appHistory from './components/app-history'
import Push from 'push.js'
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
      previousTimerState: 0,
      timerElapsedSeconds: 0, // Elapsed seconds
      timerInterval: null, // Holds interval object,
      timerTimeout: null, // Holds timeout object
      settingsVisible: false, // Settings section switch
      workSessionMinutes: 0.1, // Config default
      breakSessionMinutes: 0.2, // Config default
      completedSessionsCount: 4
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
    previouslyIdle: function () {
      return this.previousTimerState === 0
    },
    previouslyWorking: function () {
      return this.previousTimerState === 1
    },
    previouslyOnBreak: function () {
      return this.previousTimerState === 2
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
        if (this.previouslyIdle || this.previouslyOnBreak) {
          return 'Work'
        } else {
          return 'Break'
        }
      } else if (this.isWorking) {
        let secondsLeftTotal = this.workSessionMinutes * 60 - this.timerElapsedSeconds
        let minutesLeft = Math.floor(secondsLeftTotal / 60)
        let secondsLeft = secondsLeftTotal % 60
        return (minutesLeft > 9 ? minutesLeft : '0' + minutesLeft) + ':' + (secondsLeft > 9 ? secondsLeft : '0' + secondsLeft)
      } else if (this.isOnBreak) {
        let secondsLeftTotal = this.breakSessionMinutes * 60 - this.timerElapsedSeconds
        let minutesLeft = Math.floor(secondsLeftTotal / 60)
        let secondsLeft = secondsLeftTotal % 60
        return (minutesLeft > 9 ? minutesLeft : '0' + minutesLeft) + ':' + (secondsLeft > 9 ? secondsLeft : '0' + secondsLeft)
      }
    }
  },
  methods: {
    primaryAction: function () {
      var self = this
      if (this.isIdle) {
        if (this.previouslyIdle || this.previouslyOnBreak) {
          // Start working
          this.previousTimerState = this.timerState
          this.timerState = 1
          this.timerElapsedSeconds = 0
          this.timerInterval = setInterval(function () {
            self.timerElapsedSeconds += 1
          }, 1000)
          this.timerTimeout = setTimeout(function () {
            self.previousTimerState = self.timerState
            self.timerState = 0
            clearInterval(self.timerInterval)
            clearTimeout(self.timerTimeout)
            self.completedSessionsCount++
            self.notifyNewState()
          }, 1000 * 60 * this.workSessionMinutes)
        } else {
          // Start break
          this.previousTimerState = this.timerState
          this.timerState = 2
          this.timerElapsedSeconds = 0
          this.timerInterval = setInterval(function () {
            self.$nextTick(function () {
              this.timerElapsedSeconds++
            })
          }, 1000)
          this.timerTimeout = setTimeout(function () {
            self.previousTimerState = self.timerState
            self.timerState = 0
            clearInterval(self.timerInterval)
            clearTimeout(self.timerTimeout)
            self.notifyNewState()
          }, 1000 * 60 * this.breakSessionMinutes)
        }
      } else {
        this.previousTimerState = 0
        this.timerState = 0
        clearInterval(this.timerInterval)
        clearTimeout(this.timerTimeout)
      }
    },
    settingsAction: function () {
      this.settingsVisible = !this.settingsVisible
    },
    settingsCancel: function () {
      this.settingsVisible = false
    },
    settingsSave: function (data) {
      this.workSessionMinutes = data.workSessionMinutes
      this.breakSessionMinutes = data.breakSessionMinutes
      this.settingsVisible = false
    },
    notifyNewState: function () {
      if (this.previouslyWorking && this.isIdle) {
        this.pushNotificaton(
          'Totomato',
          'Time to take a break!'
        )
      }
      if (this.previouslyOnBreak && this.isIdle) {
        this.pushNotificaton(
          'Totomato',
          'Time to get back to work!'
        )
      }
    },
    pushNotificaton: function (title, body, icon) {
      Push.create(title || '{title}', {
        body: body || '{body}',
        icon: icon ? {
          x16: icon + '-x16.png',
          x32: icon + '-x32.png'
        } : {},
        timeout: 5000
      })
    }
  },
  components: {
    appHeader,
    appTimer,
    appSettings,
    appHistory
  }
}
</script>

<style>
body {
  background: #f6f6f6;
  margin: 0;
  width: 100%;
  height: 100%;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  width: 100%;
  height: 100%;
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
button {
  cursor: pointer;
}
button:focus {
  outline: 0;
}
</style>
