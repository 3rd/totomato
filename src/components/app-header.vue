<template>
  <div class="app-header">
    <div class="status">
      {{ timerStateText }}
    </div>
    <div class="settings">
      <button @click="settingsAction">Settings</button>
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
    height: 40px;
    border-bottom: 1px solid #ccc;
  }
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
