<template>
  <div>
    <h3>Tank Control System</h3>
    <div v-if="state['input']">
      <vui-item :balance="0.65" label="Input:">
        <span v-if="state['input'].power">Injecting</span>
        <span v-else>On Hold</span>&nbsp;
        <vui-button :params="{ in_toggle_injector: 1 }" icon="power-off">Toggle Power</vui-button>
      </vui-item>
      <vui-item :balance="0.65" label="Flow Rate Limit:">{{ state['input'].rate }} L/s</vui-item>
      <vui-item :balance="0.65" label="Command:">
        <vui-input-numeric
          @keypress.enter="s({ in_set_flowrate: state['input'].setrate })"
          width="3em"
          :button-count="3"
          v-model="state['input'].setrate"
          :max="state.maxrate"
        />
        <br >
        <vui-button push-state :params="{ in_set_flowrate: state['input'].setrate }">Set Flow Rate</vui-button>
      </vui-item>
    </div>
    <vui-button v-else :params="{ in_refresh_status: 1 }">Search for input port</vui-button>
    <div style="margin-top: 2em;" v-if="state['output']">
      <vui-item :balance="0.65" label="Output:">
        <span v-if="state['output'].power">Open</span>
        <span v-else>On Hold</span>&nbsp;
        <vui-button :params="{ out_toggle_power: 1 }" icon="power-off">Toggle Power</vui-button>
      </vui-item>
      <vui-item :balance="0.65" label="Max Output Pressure:">{{ state['output'].pressure }} kPa</vui-item>
      <vui-item :balance="0.65" label="Command:">
        <vui-input-numeric
          @keypress.enter="s({ out_set_pressure: state['output'].setpressure })"
          width="5em"
          :button-count="4"
          :decimal-places="2"
          v-model="state['output'].setpressure"
          :max="state.maxpressure"
        />
        <br >
        <vui-button
          push-state
          :params="{ out_set_pressure: state['output'].setpressure }"
        >Set Pressure</vui-button>
      </vui-item>
    </div>
    <vui-button v-else :params="{ out_refresh_status: 1 }">Search for output port</vui-button>
  </div>
</template>

<script>
import Utils from "@/utils"
export default {
  data() {
    return this.$root.$data
  },
  methods: {
    s(parameters) {
      Utils.sendToTopic(parameters)
    }
  }
};
</script>