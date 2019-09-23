<template>
  <div>
    <Transport @play-function="playFunction" @stop-function="stopFunction" />
    <Track
      @box-click="trackClick($event, trackArrays[0])"
      @mute-click="trackMute(1)"
      @edit-click="editTrack(1)"
      :trackArray="trackArrays[0]"
    />
    <Track
      @box-click="trackClick($event, trackArrays[1])"
      @mute-click="trackMute(2)"
      @edit-click="editTrack(2)"
      :trackArray="trackArrays[1]"
    />
    <Track
      @box-click="trackClick($event, trackArrays[2])"
      @mute-click="trackMute(3)"
      @edit-click="editTrack(3)"
      :trackArray="trackArrays[2]"
    />
    <Counter :position="position" />
    <Edit :volumeArrays="volumeArrays" :currentTrack="currentTrack" />
  </div>
</template>

<script>
import Transport from "./components/Transport.vue";
import Track from "./components/Track.vue";
import Counter from "./components/Counter.vue";
import Edit from "./components/Edit.vue";

export default {
  name: "App",
  components: {
    Transport,
    Track,
    Counter,
    Edit
  },
  data() {
    return {
      volumeArrays: [[], [], []],

      trackArrays: [[], [], []],
      track1Array: [],
      track2Array: [],
      track3Array: [],
      track1File: new Audio(require("./sounds/snare.mp3")),
      track2File: new Audio(require("./sounds/kick.mp3")),
      track3File: new Audio(require("./sounds/hi-hat.mp3")),
      position: 0,
      mutedTracks: [],
      loop: null,
      speed: 14,
      playing: false,
      currentTrack: 1
    };
  },
  mounted() {
    class Beat {
      constructor(time, volume, index) {
        (this.time = time), (this.volume = volume), (this.index = index);
      }
    }
    for (let n = 0; n < 32; n++) {
      this.trackArrays[0].push(new Beat(null, 80, n));
      this.trackArrays[1].push(new Beat(null, 80, n));
      this.trackArrays[2].push(new Beat(null, 80, n));
      this.volumeArrays[0].push(80);
      this.volumeArrays[1].push(80);
      this.volumeArrays[2].push(80);
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
              this.track1Play();
            }
          });
          this.trackArrays[1].forEach(beat => {
            if (beat.time == elapsed) {
              this.track2Play();
            }
          });
          this.trackArrays[2].forEach(beat => {
            if (beat.time == elapsed) {
              this.track3Play();
            }
          });

          elapsed++;

          if (elapsed == 321) {
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

    track1Play() {
      this.track1File.currentTime = 0;
      this.track1File.volume = this.volumeArrays[0][this.position] / 100;
      this.mutedTracks.includes(1) ? null : this.track1File.play();
    },
    track2Play() {
      this.track2File.currentTime = 0;
      this.track2File.volume = this.volumeArrays[1][this.position] / 100;

      this.mutedTracks.includes(2) ? null : this.track2File.play();
    },
    track3Play() {
      this.track3File.currentTime = 0;
      this.track3File.volume = this.volumeArrays[2][this.position] / 100;

      this.mutedTracks.includes(3) ? null : this.track3File.play();
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
      this.currentTrack = e;
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
