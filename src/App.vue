<template>
  <MenuOverlay ref="MenuOverlay"></MenuOverlay>
  <GameFrame ref="GameFrame"></GameFrame>
  <WebcamFrame ref="WebcamFrame"></WebcamFrame>
</template>

<script>
import WebcamFrame from './components/WebcamFrame.vue'
import GameFrame from './components/GameFrame.vue'
import MenuOverlay from './components/MenuOverlay.vue';

export default {
  name: 'App',
  components: {
    WebcamFrame,
    GameFrame,
    MenuOverlay
  },
  data() {
    return {
      webcamSize: {
        width: 0,
        height: 0
      }
    }
  },
  methods: {
    start () {
      this.$refs.MenuOverlay.$el.style.display = 'none'
      this.$refs.WebcamFrame.startWebcam()
    },
    startGame() {
      this.$refs.GameFrame.active = true
      this.$refs.GameFrame.startDrawing()
    }
  }
}
</script>

<style>
* { 
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
html, body {
  min-height: 100vh;
}
body {
  margin: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  position: relative;
  width: 100%;
  height: 100vh;
}
</style>
