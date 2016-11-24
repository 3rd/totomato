<template>
  <div class="app-settings">
    <h1>Totomato settings</h1>
    <div class="field">
      Work sessions last <input v-model="settingWorkSessionMinutes"> minutes.
    </div>
    <div class="field">
      Break session last <input v-model="settingBreakSessionMinutes"> minutes.
    </div>
    <div class="actions">
      <button @click="saveSettings">Save settings</button>
      <button @click="cancel">Cancel</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'app-settings',
  props: [
    'workSessionMinutes',
    'breakSessionMinutes'
  ],
  data: function () {
    return {
      settingWorkSessionMinutes: this.workSessionMinutes,
      settingBreakSessionMinutes: this.breakSessionMinutes
    }
  },
  methods: {
    saveSettings: function () {
      this.$emit('settings-save', {
        workSessionMinutes: this.settingWorkSessionMinutes,
        breakSessionMinutes: this.settingBreakSessionMinutes
      })
    },
    cancel: function () {
      this.$emit('settings-cancel')
    }
  }
}
</script>

<style scoped>
  .app-settings {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: #f6f6f6;
    padding-top: 100px;
    box-sizing: border-box;
  }
  h1 {
    font-size: 24px;
    color: #666;
    margin-bottom: 50px;
  }
  .field {
    margin-bottom: 20px;
    background: white;
    padding: 10px;
    color: #999;
    max-width: 400px;
    display: block;
    margin: auto;
    margin-bottom: 10px;
  }
  input {
    border: 1px solid #eee;
    padding: 4px;
    width: 40px;
    text-align: center;
  }
  .actions {
    margin-top: 20px;
  }
  button {
    padding: 10px 20px;
    border: 0;
    min-width: 140px;
    background: #3498db;
    color: white;
    font-size: 16px;
    margin-right: 10px;
  }
  button:last-child {
    margin-right: 0;
  }
</style>
