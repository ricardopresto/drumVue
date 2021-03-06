<template>
  <div>
    <div id="tracks">
      <Track
        v-for="(n, track) in totalTracks"
        :key="track"
        @box-click="trackClick($event, trackArrays[track])"
        @mute-click="trackMute(track)"
        @edit-click="editTrack(track)"
        :trackArray="trackArrays[track]"
        :trackName="trackNames[track]"
        :class="{ selected: currentTrackNumber == track }"
        class="track"
      />
    </div>
    <div id="controlBox">
      <div id="buttons">
        <Controls
          @play-function="playFunction"
          @pause-function="pauseFunction"
          @stop-function="stopFunction"
          @reset-function="resetFunction"
          @speed-change="speedChange($event)"
          @length-change="lengthChange($event)"
          :paused="paused"
        />
      </div>
      <div id="countAndEdit">
        <Counter :position="position" :length="length" />
        <Edit
          :currentTrack="trackArrays[currentTrackNumber]"
          :currentTrackNumber="currentTrackNumber"
          @volume-change="volChange($event)"
          @time-change="timeChange($event)"
        />
        <div>
          <TempoControl
            @speed-change="speedChange($event)"
            @length-change="lengthChange($event)"
            :length="length"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Controls from "./components/Controls.vue";
import Track from "./components/Track.vue";
import Counter from "./components/Counter.vue";
import Edit from "./components/Edit.vue";
import TempoControl from "./components/TempoControl.vue";
import demoArray from "./components/demoArray";
import { Howl, Howler } from "howler";

class Beat {
  constructor(time, timeShifted, volume, index) {
    (this.time = time),
      (this.timeShifted = timeShifted),
      (this.volume = volume),
      (this.index = index);
  }
}

export default {
  name: "App",
  components: {
    Controls,
    Track,
    Counter,
    Edit,
    TempoControl
  },
  data() {
    return {
      totalTracks: 8,
      trackArrays: [],
      trackNames: [
        "Hi-Hat Closed",
        "Hi-Hat Open",
        "Crash",
        "Cowbell",
        "Snare",
        "Hi Tom",
        "Mid Tom",
        "Kick"
      ],
      audioFiles: [
        new Howl({ src: [require("./sounds/hat-closed.mp3")] }),
        new Howl({ src: [require("./sounds/hat-open.mp3")] }),
        new Howl({ src: [require("./sounds/crash.mp3")] }),
        new Howl({ src: [require("./sounds/cowbell.mp3")] }),
        new Howl({ src: [require("./sounds/snare.mp3")] }),
        new Howl({ src: [require("./sounds/hitom.mp3")] }),
        new Howl({ src: [require("./sounds/midtom.mp3")] }),
        new Howl({ src: [require("./sounds/kick.mp3")] })
      ],
      length: 16,
      position: 0,
      mutedTracks: {},
      loop: null,
      speed: 14,
      playing: false,
      paused: false,
      currentTrackNumber: 0
    };
  },
  mounted() {
    for (let arr = 0; arr < this.totalTracks; arr++) {
      this.mutedTracks[arr] = false;
    }
    this.trackArrays = demoArray;
  },
  methods: {
    initialize() {
      for (let arr = 0; arr < this.totalTracks; arr++) {
        this.trackArrays.push([]);
        this.mutedTracks[arr] = false;
        for (let n = 0; n < this.length; n++) {
          this.trackArrays[arr].push(new Beat(null, 0, 80, n));
        }
      }
    },
    sleep(ms) {
      return new Promise(resolve => setTimeout(resolve, ms));
    },
    async playFunction() {
      if (!this.playing) {
        let elapsed = 0;
        this.playing = true;
        while (this.playing) {
          if (elapsed % 10 == 0) {
            this.position = elapsed / 10;
          }
          for (let arr = 0; arr < this.totalTracks; arr++) {
            this.trackArrays[arr].forEach(beat => {
              if (beat.time == elapsed && this.mutedTracks[arr] == false) {
                this.trackPlay(arr, beat.volume);
              }
            });
          }
          this.paused ? null : elapsed++;

          if (elapsed == this.length * 10) {
            elapsed = 0;
            this.position = 0;
          }
          await this.sleep(this.speed);
        }
      }
    },
    pauseFunction() {
      if (this.playing) {
        this.paused = !this.paused;
      }
    },
    stopFunction() {
      this.position = 0;
      this.playing = false;
      this.paused = false;
    },
    trackPlay(track, vol) {
      this.audioFiles[track].seek(0);
      this.audioFiles[track].volume(vol / 100);
      this.audioFiles[track].play();
    },
    trackClick(index, trackArray) {
      if (trackArray[index].time == null) {
        trackArray[index].time = index * 10;
      } else {
        trackArray[index].time = null;
      }
    },
    trackMute(m) {
      this.mutedTracks[m] = !this.mutedTracks[m];
    },
    editTrack(e) {
      this.currentTrackNumber = e;
    },
    volChange(volumeObject) {
      this.trackArrays[this.currentTrackNumber].forEach(beat => {
        if (beat.index == volumeObject.index) {
          beat.volume = volumeObject.volume;
          console.log(volumeObject);
        }
      });
    },
    timeChange(timeObject) {
      this.trackArrays[this.currentTrackNumber].forEach(beat => {
        if (beat.index == timeObject.index) {
          beat.timeShifted = beat.timeShifted + timeObject.time;
          beat.time = beat.time + timeObject.time;
          if (beat.time == -1) {
            beat.time = this.length * 10 - 1;
          }
          if (beat.time == this.length * 10) {
            beat.time = 0;
          }
        }
      });
    },
    speedChange(change) {
      this.speed = 36 - change;
    },
    resetFunction() {
      for (let arr = 0; arr < this.totalTracks; arr++) {
        for (let n = 0; n < this.length; n++) {
          this.trackArrays[arr][n].time = null;
          this.trackArrays[arr][n].timeShifted = 0;
          this.trackArrays[arr][n].volume = 80;
          this.currentTrackNumber = null;
          this.$nextTick(() => {
            this.currentTrackNumber = 0;
          });
        }
      }
    },
    lengthChange(len) {
      if (len != this.length) {
        this.stopFunction();
        if (this.length == 16) {
          for (let arr = 0; arr < this.totalTracks; arr++) {
            this.trackArrays.push([]);
            for (let n = 16; n < 32; n++) {
              this.trackArrays[arr].push(new Beat(null, 0, 80, n));
            }
          }
        } else {
          for (let arr = 0; arr < this.totalTracks; arr++) {
            this.trackArrays[arr].splice(16);
          }
        }
        this.length = len;
      }
    }
  }
};
</script>

<style>
* {
  padding: 0;
  margin: 0;
  font-family: Arial, Helvetica, sans-serif;
}
body {
  background-color: #383838;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}
.selected {
  background-color: #4b4b50;
}
#tracks {
  margin: 20px;
  border: 1px solid slateblue;
  border-radius: 10px;
  padding: 10px 5px 10px 10px;
  width: min-content;
}
#controlBox {
  display: flex;
  flex-direction: row;
  width: min-content;
}
#buttons {
  width: min-content;
}
#countAndEdit {
  margin-left: 20px;
}
.track {
  border: 1ps solid #fff;
  border-radius: 6px;
}
</style>
