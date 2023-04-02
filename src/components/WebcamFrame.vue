<template>
  <div id="webcam">
    <video ref="video"></video>
  </div>
</template>

<script>
require('@tensorflow/tfjs')
const handpose = require('@tensorflow-models/handpose')
export default {
  name: 'WebcamFrame',
  data() {
    return {
      model: null
    }
  },
  methods: {
    stopWebcam () {
      const video = this.$refs.video
      const stream = video.srcObject
      const tracks = stream.getTracks()
      tracks.forEach(track => {
        track.stop()
      })
      video.srcObject = null
    },
    startWebcam () {
      const video = this.$refs.video
      navigator.mediaDevices.getUserMedia({ video: true })
        .then(stream => {
          video.srcObject = stream
          video.onloadedmetadata = () => {
            video.play()
            this.$root.webcamSize.width = video.videoWidth
            this.$root.webcamSize.height = video.videoHeight
            this.$root.startGame();
            this.model = handpose.load()

            // setInterval(() => {
            this.detectMotion();
            // }, 2000);
          }
        })
        .catch(err => {
          console.log('Error: ', err)
        })
    },
    async detectMotion () {
      const video = this.$refs.video
      await this.model.then(model => {
        model.estimateHands(video).then(predictions => {
          if( predictions.length ) {
            this.$root.$refs.GameFrame.moveCharacter(predictions[0].annotations.indexFinger[0]);
          }
          requestAnimationFrame(() => {
            this.detectMotion();
          })
        })
      });
    }
  },
}
</script>

<style scoped>
#webcam {
  position: absolute;
  bottom: 0;
  right: 0;
  width: 265px;
  height: 200px;
  z-index: 99;
}
#webcam video {
  width: 100%;
  height: 100%;
  transform: scaleX(-1);
}
</style>
