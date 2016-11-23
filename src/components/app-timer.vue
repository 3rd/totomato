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
    return {
      // prop
    }
  },
  methods: {
    primaryAction: function () {
      this.$emit('action')
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
    font-size: 90px;
    border-radius: 40vw;
    margin: auto;
    margin-top: 10vw;
  }
  .circle .text {
    line-height: 40vw;
  }
</style>
