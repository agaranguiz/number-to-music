<script>
import * as Tone from 'tone';
export default {
  data() {
    return {
      number: 0,
      numberB7: 0,
      chords: []
    }
  },
  computed: {
    chordsString() {
      let string = "";
      for (let chord of this.chords) {
        string += chord + " - ";
      }
      return string.slice(0, -3);
    }
  },
  methods: {
    convert: function () {
      this.chords = [];
      let numberFloat = parseFloat(this.number);
      let scale = ['B', 'C', 'D', 'E', 'F', 'G', 'A'];      
      if(typeof(numberFloat) == 'number') {
        if(numberFloat < 0) {
          numberFloat *= -1;
        }
        this.numberB7 = numberFloat.toString(7);
        for(let i = 0; i < this.numberB7.length; i++) {
          let index = this.numberB7.charAt(i);
          if(index == '.') {
            this.chords.push('.')
          } else {
            this.chords.push(scale[index]);
          }          
        }
      } else {
        console.log("Error")
      }
    },
    play: function() {
      const synth = new Tone.Synth().toDestination();
      const now = Tone.now();
      let speed = 2;      
      let duration = "8n";
      let count = 0;
      for (let chord of this.chords) {
        if(chord == '.') {
          synth.triggerAttackRelease("C1", "0", now + count/speed);
          speed = 4;
          duration = "16n";
        } else {
          synth.triggerAttackRelease(chord + "4", duration, now + count/speed);
        }
        count++;     
      }
    }
  },
  mounted() {
    
  }
}
</script>
<template>
<el-container>
  <el-header>Type a number, convert it, then click 'Play' to hear what it sounds like</el-header>
  <el-main>
    <el-row :gutter="20" justify="center">
      <el-col :span="6">
        <span>Number</span>     
      </el-col>
      <el-col :span="18">  
        <el-tooltip content="Enter a number">
          <el-input-number :controls="false" v-model="number"></el-input-number>
        </el-tooltip>          
      </el-col>
    </el-row>
    <el-row :gutter="20">
      <el-col :span="6">
          <span>Base 7</span>    
      </el-col>
      <el-col :span="18">
        <el-tooltip content="The number converted to base 7. We use the NNS notation on the key of C">
          <span> {{ numberB7 }} </span>
        </el-tooltip>        
      </el-col>
    </el-row>
    <el-row :gutter="20">
      <el-col :span="6">
        <span>To letter notation</span>
      </el-col>
      <el-col :span="18">
        <el-tooltip content="We take the new number convert it to letter notation">
          <span> {{ chordsString }} </span>
        </el-tooltip>        
      </el-col>
    </el-row>
    <el-row :gutter="20">
      <el-col :span="8">
        <el-button @click="convert" type="primary">Convert</el-button>
      </el-col>
      <el-col :span="8">
        <el-button @click="play" type="success">Play</el-button>
      </el-col>
    </el-row>
  </el-main>
</el-container>
</template>
<style>
.el-row {
  margin-bottom: 20px;
}
.el-row:last-child {
  margin-bottom: 0;
}
.el-col {
  border-radius: 4px;
}
.tooltip-base-box {
  width: 600px;
}
.tooltip-base-box .row {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.tooltip-base-box .center {
  justify-content: center;
}
.tooltip-base-box .box-item {
  width: 210px;
  margin-top: 10px;
}
</style>