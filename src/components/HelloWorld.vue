<template>
  <div>
    <button @click="beginDetect2">Start</button>
    <p>Volume:</p>
    <span id="audio-value"></span>
    <div class="container-level-bar">
      <div class="level-bar"></div>
      <div class="level-bar"></div>
      <div class="level-bar"></div>
      <div class="level-bar"></div>
      <div class="level-bar"></div>
      <div class="level-bar"></div>
      <div class="level-bar"></div>
      <div class="level-bar"></div>
      <div class="level-bar"></div>
      <div class="level-bar"></div>
    </div>
  </div>
</template>

<script>
var audioContext;
var mediaStreamSource = null;

var meter = null

import createAudioMeter from "../utils/volume-meter";
import createAudioMeter2 from "../utils/volume-meter2";

export default {
  name: "HelloWorld",
  props: {
    msg: String
  },
  data: () => {
    return {
      volume: 0
    };
  },
  methods: {
    beginDetect() {
      audioContext = new (window.AudioContext || window.webkitAudioContext)();
      if (navigator.mediaDevices && navigator.mediaDevices.getUserMedia) {
        navigator.mediaDevices.getUserMedia({ audio: true }).then(stream => {
          mediaStreamSource = audioContext.createMediaStreamSource(stream);
          // meter = this.createAudioMeter(audioContext);
          meter = createAudioMeter(audioContext);
          meter.volume = 1;
          mediaStreamSource.connect(meter);
          document.getElementById("audio-value").innerHTML = meter.volume;
          // debugger;
        });
      }
    },
    async beginDetect2() {
      var stream = await navigator.mediaDevices.getUserMedia({ audio: true });

      let meter = createAudioMeter2(stream)
    }
    // createAudioMeter(audioContext, clipLevel, averaging, clipLag) {

    //   const volumeAudioProcess = function(event) {
    //   const buf = event.inputBuffer.getChannelData(0);
    //   const bufLength = buf.length;
    //   let sum = 0;
    //   let x;

    //   // Do a root-mean-square on the samples: sum up the squares...
    //   for (var i = 0; i < bufLength; i++) {
    //     x = buf[i];
    //     if (Math.abs(x) >= this.clipLevel) {
    //       this.clipping = true;
    //       this.lastClip = window.performance.now();
    //     }
    //     sum += x * x;
    //   }

    //   // ... then take the square root of the sum.
    //   const rms = Math.sqrt(sum / bufLength);

    //   // Now smooth this out with the averaging factor applied
    //   // to the previous sample - take the max here because we
    //   // want "fast attack, slow release."
    //   this.volume = Math.max(rms, this.volume * this.averaging);
    //   document.getElementById("audio-value").innerHTML = this.volume;
    // }

    //   const processor = audioContext.createScriptProcessor(512);
    //   processor.onaudioprocess = volumeAudioProcess;
    //   processor.clipping = false;
    //   processor.lastClip = 0;
    //   processor.volume = 0;
    //   processor.clipLevel = clipLevel || 0.98;
    //   processor.averaging = averaging || 0.95;
    //   processor.clipLag = clipLag || 750;

    //   // this will have no effect, since we don't copy the input to the output,
    //   // but works around a current Chrome bug.
    //   processor.connect(audioContext.destination);

    //   processor.checkClipping = function() {
    //     if (!this.clipping) {
    //       return false;
    //     }
    //     if (this.lastClip + this.clipLag < window.performance.now()) {
    //       this.clipping = false;
    //     }
    //     return this.clipping;
    //   };

    //   processor.shutdown = function() {
    //     this.disconnect();
    //     this.onaudioprocess = null;
    //   };

    //   return processor;
    // },
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container-level-bar {
}

div[class^="level-bar"] {
  display: inline-block;
  width: 20px;
  height: 20px;
  background-color: seagreen;
  margin: 2px;
}
</style>
