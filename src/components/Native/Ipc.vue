<template>
  <div v-if="false"></div>
</template>

<script>
import { mapState } from 'vuex'
import {
  commands
} from 'components/Command/index'

export default {
  name: 'mo-ipc',
  computed: {
    ...mapState('preference', {
      enableEggFeatures: state => state.config.enableEggFeatures
    })
  },
  watch: {
  },
  methods: {
    bindIpcEvents: function () {
      this.$q.electron.ipcRenderer.on('command', (event, command, ...args) => {
        commands.execute(command, ...args)
      })
    },
    unbindIpcEvents: function () {
      this.$q.electron.ipcRenderer.removeAllListeners('command')
    }
  },
  created: function () {
    this.bindIpcEvents()
    // id of the menu item
    let visibleStates = {}
    // if (is.mas()) {
    //   visibleStates['app.check-for-updates'] = false
    //   if (!this.enableEggFeatures) {
    //     visibleStates['task.new-bt-task'] = false
    //   }
    // }
    this.$q.electron.ipcRenderer.send('update-menu-states', visibleStates, null, null)
  },
  destroyed: function () {
    this.unbindIpcEvents()
  }
}
</script>
