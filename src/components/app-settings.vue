<template>
  <div class="app-settings">
    <h1>Totomato settings</h1>
    <div class="field">
      Work sessions last <input v-model="settingWorkSessionMinutes"> minutes.
    </div>
    <div class="field">
      Break session last <input v-model="settingBreakSessionMinutes"> minutes.
    </div>
    <button @click="saveSettings">Save settings</button>
    <button @click="cancel">Cancel</button>
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
  }
  input {
    border: 1px solid #eee;
    padding: 4px;
    width: 40px;
    text-align: center;
  }
  .field {
    margin-bottom: 20px;
  }
</style>
