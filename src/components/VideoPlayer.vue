<template>
  <div class="video">
    <video
        ref="video"
        @mouseover="isClickBar = true"
        @mouseleave="isClickBar = false"
        @click="videoButton"
        src="../../src/videos/MORGENSHTERN-12.mp4">
    </video>
    <div
        ref="click-bar"
        @mouseover="isClickBar = true"
        @mouseleave="isClickBar = false"
        :class="{
      'open': isClickBar
        }"
        class="click-bar">
      <div
          @click="videoButton"
          class="click-bar__play">
        <div
            v-if="isPlaying === false"
            class="play">
          <img src="../../src/images/play.png" alt="icon">
        </div>
        <div
            v-if="isPlaying === true"
            class="stop">
          <img src="../../src/images/pause.png" alt="icon">
        </div>
      </div>
      <div class="click-bar__progress">
        <Slider
            v-model="value"
            :max="max"
            :lazy="false"
            :step="0.01"
            :tooltips="false"
            @change="clickTrack"
            :format="formatTime"
            class="custom-slider"
        />
      </div>
      <div class="click-bar__time">
        {{ currentTimeFormatted !== '' ? currentTimeFormatted : '00:00' }} / {{ durationTime !== '' ? durationTime : '00:00' }}
      </div>
      <div
          @mouseover="soundBar = true"
          @mouseleave="soundBar = false"
          :class="{
          'open': soundBar && isSound
            }"
          class="click-bar__sound">
        <div class="sound-track">
          <Slider
              v-model="valueSound"
              :max="100"
              :lazy="false"
              :tooltips="false"
              @update="clickSound"
              class="custom-sound"
          />
        </div>
        <div
            @click="clickSoundButton"
            class="sound-icon">
          <img v-if="isSound" src="../../src/images/sound.png" alt="icon">
          <img v-else src="../../src/images/no-sound.png" alt="icon">
        </div>
      </div>
      <div
          @click="fullScreen"
          class="click-bar__fullscreen">
        <img src="../../src/images/fullscreen.png" alt="icon">
      </div>
    </div>
  </div>
</template>

<script>
import Slider from '@vueform/slider'
import "@vueform/slider/themes/default.css"
export default {
  components: {
    Slider
  },
  data() {
    return {
      currentTimeFormatted: '',
      isPlaying: false,
      durationTime: '',
      currentTime: false,
      isClickBar: true,
      value: 0,
      valueSound: 50,
      max: 0,
      isSound: true,
      soundBar: false
    }
  },
  props: {
    videoLink: {
      type: String,
      required: true
    }
  },
  methods: {
    videoButton() {
      if (this.isPlaying) {
        this.$refs.video.pause()
        this.isPlaying = false
      } else {
        this.$refs.video.play()
        this.isPlaying = true
      }
    },
    endPlayer() {
      this.$refs.video.pause()
      this.currentTime = 0
      this.isPlaying = false
    },
    clickSound(value) {
      this.isSound = true
      this.$refs.video.volume = value / 100
      if (value === 0) {
        this.isSound = false
      }
    },
    fullScreen() {
      let container = document.querySelector('.container')
      if (container) {
        container.classList.toggle('full')
      }
    },
    clickSoundButton() {
      this.isSound = !this.isSound
      this.$refs.video.muted = !this.isSound
    },
    draggingProgress(value) {
      console.log(value)
    },
    dragStart() {
      this.$refs.video.pause()
      this.isPlaying = false
    },
    dragEnd() {
      this.$refs.video.play()
      this.isPlaying = true
    },
    clickTrack(value) {
      this.valuformatTime(time) {
        let hours = Math.floor(time / 60 / 60);
        let minutes = Math.floor(time / 60) - (hours * 60);
        let seconds = Math.floor(time % 60);
        let formatted = '00:00:00'
        if (hours > 0) {
          formatted = [
            hours.toString().padStart(2, '0'),
            minutes.toString().padStart(2, '0'),
            seconds.toString().padStart(2, '0')
          ].join(':');
        } else {
          formatted = [
            minutes.toString().padStart(2, '0'),
            seconds.toString().padStart(2, '0')
          ].join(':');
        }
        return formatted
      },e = value
      this.currentTime = value
    },

    updateProgress(e) {
      let { currentTime, duration } = e.srcElement;
      if (this.currentTime !== false) {
        this.$refs.video.currentTime = this.currentTime;
        currentTime = this.$refs.video.currentTime
        this.currentTime = false
      }
      this.max = Math.floor(duration)
      this.value = currentTime
      let hours = Math.floor(currentTime / 60 / 60);
      let minutes = Math.floor(currentTime / 60) - (hours * 60);
      let seconds = Math.floor(currentTime % 60);
      let durationHours = Math.floor(duration / 60 / 60)
      let durationMinutes = Math.floor(duration / 60) - durationHours * 60
      let durationSeconds = Math.floor(duration % 60);
      let formatted = '00:00:00'
      let formattedDuration = '00:00:00'
      if (durationHours > 0) {
        formatted = [
          hours.toString().padStart(2, '0'),
          minutes.toString().padStart(2, '0'),
          seconds.toString().padStart(2, '0')
        ].join(':');
        formattedDuration = [
          durationHours.toString().padStart(2, '0'),
          durationMinutes.toString().padStart(2, '0'),
          durationSeconds.toString().padStart(2, '0')
        ].join(':');
      } else if (durationMinutes > 0) {
        formatted = [
          minutes.toString().padStart(2, '0'),
          seconds.toString().padStart(2, '0')
        ].join(':');
        formattedDuration = [
          durationMinutes.toString().padStart(2, '0'),
          durationSeconds.toString().padStart(2, '0')
        ].join(':');
      } else {
        formatted = [
          seconds.toString().padStart(2, '0'),
        ].join(':');
        formattedDuration = [
          durationSeconds.toString().padStart(2, '0')
        ].join(':');
      }
      this.currentTimeFormatted = formatted
      this.durationTime = formattedDuration
    }
  },
  mounted() {
    this.$refs.video.addEventListener('loadeddata', this.updateProgress);
    this.$refs.video.addEventListener('timeupdate', this.updateProgress);
    this.$refs.video.addEventListener('ended', this.endPlayer);
    this.$refs.video.volume = 0.5
  }
}
</script>

<style lang="scss" scoped>
.video {
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 2147483646;
  position: relative;
  overflow: hidden;
  width: 100%;
  height: 100%;
}
.custom-slider {
  --slider-bg: #464646;
  --slider-connect-bg: #fff;
  --slider-connect-bg-disabled: #9CA3AF;
  --slider-height: 5px;
  --slider-vertical-height: 300px;
  --slider-radius: 9999px;

  --slider-handle-bg: #fff;
  --slider-handle-border: 0;
  --slider-handle-width: 10px;
  --slider-handle-height: 10px;
  --slider-handle-radius: 9999px;
  --slider-handle-shadow: 0.5px 0.5px 2px 1px rgba(0,0,0,0);
  --slider-handle-shadow-active: 0.5px 0.5px 2px 1px rgba(0,0,0,0);
  --slider-handle-ring-width: 3px;
  --slider-handle-ring-color: #10B98130;
}
.custom-sound {
  --slider-bg: #464646;
  --slider-connect-bg: #fff;
  --slider-connect-bg-disabled: #9CA3AF;
  --slider-height: 5px;
  --slider-vertical-height: 300px;
  --slider-radius: 9999px;

  --slider-handle-bg: #fff;
  --slider-handle-border: 0;
  --slider-handle-width: 10px;
  --slider-handle-height: 10px;
  --slider-handle-radius: 9999px;
  --slider-handle-shadow: 0.5px 0.5px 2px 1px rgba(0,0,0,0);
  --slider-handle-shadow-active: 0.5px 0.5px 2px 1px rgba(0,0,0,0);
  --slider-handle-ring-width: 3px;
  --slider-handle-ring-color: #10B98130;
}
.click-bar__sound {
  display: flex;
  align-items: center;
  transition: all 0.3s;
  gap: 5px;
  &.open {
    padding: 0 0 0 5px;
    .sound-track {
      width: 60px;
    }
  }
  .sound-track {
    width: 0;
    overflow: hidden;
    transition: all 0.3s;
    border-radius: 5px;
    height: 5px;
    .sound-track__progress {
      background: white;
      height: 100%;
      width: 0;
    }
  }
}
.click-bar {
  position: absolute;
  display: flex;
  padding: 0 10px;
  align-items: center;
  gap: 10px;
  color: white;
  left: 0;
  bottom: 0;
  width: 100%;
  height: 40px;
  transition: all 0.3s;
  z-index: 2147483647;
  * {
    z-index: 2147483647;
  }
  background: rgba(0, 0, 0, 0.5);
  transform: translate(0, 100%);
  &.open {
    transform: translate(0, 0);
  }
}
.click-bar__play {
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
}
.click-bar__play div, .click-bar__fullscreen, .sound-icon {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 25px;
  height: 25px;
}
.click-bar__play div img, .click-bar__fullscreen img, .sound-icon img {
  max-width: 100%;
}
.click-bar__progress {
  flex: 1 1 auto;
}
.click-bar__time {
  text-align: center;
}
.video video {
  width: 100%;
  height: 100%;
  object-fit: contain;
}
</style>