<script>
import * as Tone from 'tone';
export default {
  data() {
    return {
      number: 0
    }
  },
  methods: {
    playTestSound() {
      const synth = new Tone.Synth().toDestination();
      synth.triggerAttackRelease("C4", "8n");
    }
  },
  mounted() {
    
  }
}
</script>
<template>
<el-button @click="playTestSound">Play</el-button>
</template>
<style>

</style>