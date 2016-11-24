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
  .circle {
    background: #fc5455;
    width: 40vw;
    height: 40vw;
    text-align: center;
    color: white;
    font-size: 80px;
    border-radius: 40vw;
    margin: auto;
    margin-top: 10vw;
  }
  .circle .text {
    line-height: 40vw;
  }
</style>
