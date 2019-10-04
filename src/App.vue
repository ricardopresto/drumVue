<template>
  <div>
    <Controls
      @play-function="playFunction"
      @pause-function="pauseFunction"
      @stop-function="stopFunction"
      @reset-function="resetFunction"
    />
    <Track
      v-for="(n, track) in totalTracks"
      :key="track"
      @box-click="trackClick($event, trackArrays[track])"
      @mute-click="trackMute(track)"
      @edit-click="editTrack(track)"
      :trackArray="trackArrays[track]"
      :trackName="trackNames[track]"
      :class="{selected: currentTrackNumber == track}"
    />
    <Counter :position="position" />
    <Edit
      :currentTrack="trackArrays[currentTrackNumber]"
      :currentTrackNumber="currentTrackNumber"
      @volume-change="volChange($event)"
      @time-change="timeChange($event)"
    />
  </div>
</template>

<script>
import Controls from "./components/Controls.vue";
import Track from "./components/Track.vue";
import Counter from "./components/Counter.vue";
import Edit from "./components/Edit.vue";

export default {
  name: "App",
  components: {
    Controls,
    Track,
    Counter,
    Edit
  },
  data() {
    return {
      totalTracks: 4,
      trackArrays: [],
      trackNames: ["Snare", "Kick", "Hi-hat Closed", "Hi-hat Open"],
      audioFiles: [
        new Audio(require("./sounds/snare.mp3")),
        new Audio(require("./sounds/kick.mp3")),
        new Audio(require("./sounds/hat-closed.mp3")),
        new Audio(require("./sounds/hat-open.mp3"))
      ],
      position: 0,
      mutedTracks: [],
      loop: null,
      speed: 14,
      playing: false,
      paused: false,
      currentTrackNumber: null
    };
  },
  mounted() {
    class Beat {
      constructor(time, volume, index) {
        (this.time = time), (this.volume = volume), (this.index = index);
      }
    }
    for (let arr = 0; arr < this.totalTracks; arr++) {
      this.trackArrays.push([]);
      for (let n = 0; n < 32; n++) {
        this.trackArrays[arr].push(new Beat(null, 80, n));
      }
    }
  },
  methods: {
    playFunction() {
      let elapsed = 0;
      if (!this.playing) {
        this.loop = setInterval(() => {
          if (elapsed % 10 == 0) {
            this.position = elapsed / 10;
          }
          for (let arr = 0; arr < this.totalTracks; arr++) {
            this.trackArrays[arr].forEach(beat => {
              if (beat.time == elapsed) {
                this.trackPlay(arr, beat.volume);
              }
            });
          }
          this.paused ? null : elapsed++;

          if (elapsed == 320) {
            elapsed = 0;
            this.position = 0;
          }
        }, this.speed);
      }
      this.playing = true;
    },
    pauseFunction() {
      this.paused = !this.paused;
    },
    stopFunction() {
      clearInterval(this.loop);
      this.position = 0;
      this.playing = false;
      this.paused = false;
    },
    trackPlay(track, vol) {
      this.audioFiles[track].currentTime = 0;
      this.audioFiles[track].volume = vol / 100;
      this.mutedTracks.includes(track) ? null : this.audioFiles[track].play();
    },
    trackClick(index, trackArray) {
      if (trackArray[index].time == null) {
        trackArray[index].time = index * 10;
      } else {
        trackArray[index].time = null;
      }
    },
    trackMute(m) {
      if (this.mutedTracks.includes(m)) {
        this.mutedTracks = this.mutedTracks.filter(i => i != m);
      } else {
        this.mutedTracks.push(m);
      }
    },
    editTrack(e) {
      this.currentTrackNumber = e;
    },
    volChange(volumeObject) {
      this.trackArrays[this.currentTrackNumber].forEach(beat => {
        if (beat.index == volumeObject.index) {
          beat.volume = volumeObject.volume;
        }
      });
    },
    timeChange(timeObject) {
      this.trackArrays[this.currentTrackNumber].forEach(beat => {
        if (beat.index == timeObject.index) {
          beat.time = beat.time + timeObject.time;
          if (beat.time == -1) {
            beat.time = 319;
          }
          if (beat.time == 320) {
            beat.time = 0;
          }
        }
      });
    },
    resetFunction() {
      for (let arr = 0; arr < this.totalTracks; arr++) {
        for (let n = 0; n < 32; n++) {
          this.trackArrays[arr][n].time = null;
          this.trackArrays[arr][n].volume = 80;
        }
      }
    }
  }
};
</script>

<style>
* {
  padding: 0;
  margin: 0;
}
.selected {
  background-color: lightgrey;
}
</style>
