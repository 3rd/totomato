<template>
  <div class="app-header">
    <div class="status">
      {{ timerStateText }}
    </div>
    <div class="settings">
      <button @click="settingsAction">&#9881;</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'app-header',
  props: ['timerStateText'],
  data: function () {
    return {}
  },
  methods: {
    settingsAction: function () {
      this.$emit('settings-action')
    }
  }
}
</script>

<style scoped>
  .app-header {
    box-sizing: border-box;
    padding: 10px 20px;
    width: 100%;
  }
  .status, .settings {
    vertical-align: middle;
    display: block;
  }
  .status {
    text-transform: uppercase;
    color: #ccc;
    float: left;
    font-weight: bold;
    line-height: 30px;
  }
  button {
    border: 0;
    float: right;
    font-size: 30px;
    color: #ccc;
    background: transparent;
    padding: 0;
    margin: 0;
  }
</style>
