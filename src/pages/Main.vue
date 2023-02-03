<template>
<div class="container">
  <video-player
      :videoLink="videoLink"
  />
</div>
</template>

<script>
import VideoPlayer from "@/components/VideoPlayer";
export default {
  components: {VideoPlayer},
  data() {
    return {
      videoLink: 'MORGENSHTERN-12.mp4'
    }
  }
}
</script>

<style scoped>
</style>