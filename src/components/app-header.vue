<template>
  <div class="app-header">
    <div class="status">
      {{ timerStateText }}
    </div>
    <div class="settings">
      <button>Settings</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'app-header',
  props: ['timerStateText'],
  data: function () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  }
}
</script>

<style scoped>
  .status {
    text-transform: uppercase;
    color: #ccc;
    float: left;
  }
  button {
    border: 0;
    float: right;
  }
</style>
