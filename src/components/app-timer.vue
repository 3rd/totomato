<template>
  <div class="app-timer">
    <div @click="primaryAction" class="circle">
      <div class="text">{{ timerText }}</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'app-timer',
  props: ['timerText'],
  data: function () {
    return {}
  },
  methods: {
    primaryAction: function () {
      console.log('emit')
      this.$emit('primary-action')
    }
  }
}
</script>

<style scoped>
  .app-timer {
    display: flex;
    align-items: center;
    justify-content: center;
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 0;
  }
  .circle {
    background: #fc5455;
    width: 40vw;
    height: 40vw;
    max-width: 200px;
    max-height: 200px;
    text-align: center;
    color: white;
    font-size: 40px;
    border-radius: 40vw;
    margin: auto;
    cursor: pointer;
    position: relative;
    display: flex;
    align-items: center;
    justify-content: center;
  }
</style>
