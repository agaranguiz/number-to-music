<script>
import * as Tone from 'tone';
export default {
  data() {
    return {
      number: 0,
      numberB7: 0,
      notes: []
    }
  },
  computed: {
    notesString() {
      let string = "";
      for (let note of this.notes) {
        string += "#" + note + " - ";
      }
      return string.slice(0, -3);
    }
  },
  methods: {
    convert: function () {
      this.notes = [];
      let numberFloat = parseFloat(this.number);
      let scale = ['C', 'D', 'E', 'F', 'G', 'A', 'B'];      
      if(typeof(numberFloat) == 'number') {
        if(numberFloat < 0) {
          numberFloat *= -1;
        }
        this.numberB7 = numberFloat.toString(7);
        for(let i = 0; i < this.numberB7.length; i++) {
          let index = this.numberB7.charAt(i);
          this.notes.push(scale[index]);
        }
        console.log(JSON.stringify(this.notes, null, 2));
      } else {
        console.log("Error")
      }
    },
    play: function() {
      const synth = new Tone.Synth().toDestination();
      const now = Tone.now();
      let count = 0;
      for (let note of this.notes) {
        synth.triggerAttackRelease(note + "4", "16n", now + count);
        count++;
      }
    },
    playTestSound: function () {
      const synth = new Tone.Synth().toDestination();
      synth.triggerAttackRelease("C4", "8n");
    }
  },
  mounted() {
    
  }
}
</script>
<template>
<el-container>
  <el-header>Type a number, then click 'Play' to hear what it sounds like</el-header>
  <el-main>
    <el-form>
      <el-form-item label="Number">
        <el-input-number :controls="false" v-model="number"></el-input-number>
      </el-form-item>
      <el-form-item label="Base 7">
        <el-input :controls="false" v-model="numberB7" disabled></el-input>
      </el-form-item>
      <el-form-item label="To notes">
        <el-input v-model="notesString" disabled></el-input>
      </el-form-item>
      <el-form-item>
        <el-button @click="convert">Convert</el-button>
        <el-button @click="play">Play</el-button>
      </el-form-item>
    </el-form>
  </el-main>
</el-container>
</template>
<style>

</style>