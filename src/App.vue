<template>
  <div>
    <Controls
      @play-function="playFunction"
      @stop-function="stopFunction"
      @reset-function="resetFunction"
    />
    <Track
      @box-click="trackClick($event, trackArrays[0])"
      @mute-click="trackMute(0)"
      @edit-click="editTrack(0)"
      :trackArray="trackArrays[0]"
    />
    <Track
      @box-click="trackClick($event, trackArrays[1])"
      @mute-click="trackMute(1)"
      @edit-click="editTrack(1)"
      :trackArray="trackArrays[1]"
    />
    <Track
      @box-click="trackClick($event, trackArrays[2])"
      @mute-click="trackMute(2)"
      @edit-click="editTrack(2)"
      :trackArray="trackArrays[2]"
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
      trackArrays: [[], [], []],
      track0File: new Audio(require("./sounds/snare.mp3")),
      track1File: new Audio(require("./sounds/kick.mp3")),
      track2File: new Audio(require("./sounds/hi-hat.mp3")),
      position: 0,
      mutedTracks: [],
      loop: null,
      speed: 14,
      playing: false,
      currentTrackNumber: 0
    };
  },
  mounted() {
    class Beat {
      constructor(time, volume, index) {
        (this.time = time), (this.volume = volume), (this.index = index);
      }
    }
    for (let arr = 0; arr < 3; arr++) {
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
          this.trackArrays[0].forEach(beat => {
            if (beat.time == elapsed) {
              this.track1Play(beat.volume);
            }
          });
          this.trackArrays[1].forEach(beat => {
            if (beat.time == elapsed) {
              this.track2Play(beat.volume);
            }
          });
          this.trackArrays[2].forEach(beat => {
            if (beat.time == elapsed) {
              this.track3Play(beat.volume);
            }
          });

          elapsed++;

          if (elapsed == 320) {
            elapsed = 0;
            this.position = 0;
          }
        }, this.speed);
      }
      this.playing = true;
    },

    stopFunction() {
      clearInterval(this.loop);
      this.position = 0;
      this.playing = false;
    },

    track1Play(vol) {
      this.track0File.currentTime = 0;
      this.track0File.volume = vol / 100;
      this.mutedTracks.includes(0) ? null : this.track0File.play();
    },
    track2Play(vol) {
      this.track1File.currentTime = 0;
      this.track1File.volume = vol / 100;
      this.mutedTracks.includes(1) ? null : this.track1File.play();
    },
    track3Play(vol) {
      this.track2File.currentTime = 0;
      this.track2File.volume = vol / 100;
      this.mutedTracks.includes(2) ? null : this.track2File.play();
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
      for (let arr = 0; arr < 3; arr++) {
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
</style>
