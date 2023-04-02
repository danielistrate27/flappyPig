<template>
  <canvas ref="canvas" id="canvas"></canvas>
  <div class="points" ref="points" v-if="firstPredictionDone && active">Points: {{ points }}</div>
  <div class="rise-hand" v-if="!firstPredictionDone && active">
    <h1>Rise your hand to start the game!</h1>
  </div>
  <div ref="counter" class="counter">
    <span style="display: none">3</span>
    <span style="display: none">2</span>
    <span style="display: none">1</span>
    <span style="display: none">GO!</span>
  </div>
  <div class="game-over" v-if="gameOver">
    <h1>Game Over!</h1>
    <h2>Points: {{ points }}</h2>
  </div>
</template>

<script>
export default {
  name: 'GameFrame',
  data() {
    return {
      active: false,
      firstPredictionDone: false,
      gameReady: false,
      gameOver: false,
      pipesGapSize: 150,
      points: 0,
      characterData: {
        x: 50,
        y: 0,
        width: 20,
        height: 20,
        color: 'red'
      },
      pipes: [],
      canvasContext: null,
      difficulty: 1,
      initialGameData: {
        firstPipePosition: 200,
        distanceBetweenPipes: 120,
        pipeWidth: 20,
      }
    }
  },
  computed: {
    videoWidth() {
      return this.$root.webcamSize.width
    },
    videoHeight() {
      return this.$root.webcamSize.height
    },
    canvas() {
      return this.$refs.canvas;
    }
  },
  methods: {
    startDrawing() {
      this.canvas.width = this.videoWidth || 640
      this.canvas.height = this.videoHeight || 480

      this.canvasContext = this.canvas.getContext('2d');

      // Set initial character position
      this.characterData.y = this.canvas.height/2;
      
      this.drawInitialPipes();
    },

    drawInitialPipes() {
      const firstX = this.initialGameData.firstPipePosition;
      const maxPipesCount = this.canvas.width / ( this.initialGameData.distanceBetweenPipes + this.initialGameData.pipeWidth );

      for( let i = 0; i < maxPipesCount; i++ ) {
        const x = firstX + i * ( this.initialGameData.distanceBetweenPipes + this.initialGameData.pipeWidth );
        const topPipeLength = Math.floor(Math.random() * (this.canvas.height - this.pipesGapSize))
        const bottomPipeLength = this.canvas.height - topPipeLength - this.pipesGapSize
        this.pipes.push([
          {
            x: x,
            y: 0,
            width: this.initialGameData.pipeWidth,
            height: topPipeLength
          },
          {
            x: x,
            y: topPipeLength + this.pipesGapSize,
            width: this.initialGameData.pipeWidth,
            height: bottomPipeLength
          }
        ])
      }

      this.drawScreen();
    },

    generateNewPipe() {
      const lastPipe = this.pipes[this.pipes.length - 1];
      const lastPipeX = lastPipe[0].x;

      const topPipeLength = Math.floor(Math.random() * (this.canvas.height - this.pipesGapSize))
      const bottomPipeLength = this.canvas.height - topPipeLength - this.pipesGapSize

      this.pipes.push([
        {
          x: lastPipeX + this.initialGameData.distanceBetweenPipes + this.initialGameData.pipeWidth,
          y: 0,
          width: this.initialGameData.pipeWidth,
          height: topPipeLength
        },
        {
          x: lastPipeX + this.initialGameData.distanceBetweenPipes + this.initialGameData.pipeWidth,
          y: topPipeLength + this.pipesGapSize,
          width: this.initialGameData.pipeWidth,
          height: bottomPipeLength
        }
      ])
    },

    drawPigCharacter() {
      const x = this.characterData.x; // x-coordinate of the pig's position
      const y = this.characterData.y; // y-coordinate of the pig's position

      // Draw the pig's body
      this.canvasContext.beginPath();
      this.canvasContext.arc(x, y, 30, 0, 2 * Math.PI);
      this.canvasContext.fillStyle = '#ffccd5';
      this.canvasContext.fill();
      this.canvasContext.strokeStyle = '#e8a5b7';
      this.canvasContext.stroke();

      // Draw the pig's ears
      this.canvasContext.beginPath();
      this.canvasContext.moveTo(x - 23, y - 18);
      this.canvasContext.quadraticCurveTo(x - 15, y - 35, x - 5, y - 22);
      this.canvasContext.quadraticCurveTo(x - 7, y - 13, x - 23, y - 18);
      this.canvasContext.fillStyle = '#ffccd5';
      this.canvasContext.fill();
      this.canvasContext.strokeStyle = '#e8a5b7';
      this.canvasContext.stroke();

      this.canvasContext.beginPath();
      this.canvasContext.moveTo(x + 23, y - 18);
      this.canvasContext.quadraticCurveTo(x + 15, y - 35, x + 5, y - 22);
      this.canvasContext.quadraticCurveTo(x + 7, y - 13, x + 23, y - 18);
      this.canvasContext.fillStyle = '#ffccd5';
      this.canvasContext.fill();
      this.canvasContext.strokeStyle = '#e8a5b7';
      this.canvasContext.stroke();

      // Draw the pig's eyes
      this.canvasContext.beginPath();
      this.canvasContext.arc(x - 10, y - 5, 4, 0, 2 * Math.PI);
      this.canvasContext.fillStyle = '#000000';
      this.canvasContext.fill();
      this.canvasContext.beginPath();
      this.canvasContext.arc(x + 10, y - 5, 4, 0, 2 * Math.PI);
      this.canvasContext.fillStyle = '#000000';
      this.canvasContext.fill();

      // Draw the pig's snout
      this.canvasContext.beginPath();
      this.canvasContext.moveTo(x - 12, y + 8);
      this.canvasContext.quadraticCurveTo(x - 12, y + 25, x, y + 25);
      this.canvasContext.quadraticCurveTo(x + 12, y + 25, x + 12, y + 8);
      this.canvasContext.fillStyle = '#ffccd5';
      this.canvasContext.fill();
      this.canvasContext.strokeStyle = '#e8a5b7';
      this.canvasContext.stroke();

      // Draw the pig's nostrils
      this.canvasContext.beginPath();
      this.canvasContext.arc(x - 3, y + 14, 1.5, 0, 2 * Math.PI);
      this.canvasContext.fillStyle = '#000000';
      this.canvasContext.fill();
      this.canvasContext.beginPath();
      this.canvasContext.arc(x + 3, y + 14, 1.5, 0, 2 * Math.PI);
      this.canvasContext.fillStyle = '#000000';
      this.canvasContext.fill();

      // Draw the pig's mouth
      this.canvasContext.beginPath();
      this.canvasContext.moveTo(x - 10, y + 20);
      this.canvasContext.quadraticCurveTo(x, y + 23, x + 10, y + 20);
      this.canvasContext.strokeStyle = '#e8a5b7';
      this.canvasContext.lineWidth = 2;
      this.canvasContext.stroke();
    },
    

    drawScreen() {
      this.canvasContext.clearRect(0, 0, this.canvas.width, this.canvas.height)
      this.canvasContext.fillStyle = '#008000'
      
      if( this.pipes.length ) {
        this.pipes.forEach(pipesPair => {
          pipesPair.forEach(( pipe ) => {
            this.canvasContext.beginPath();
            this.canvasContext.rect(pipe.x, pipe.y, pipe.width, pipe.height);
            this.canvasContext.fillStyle = '#008000';
            this.canvasContext.fill();
            this.canvasContext.strokeStyle = '#006400';
            this.canvasContext.lineWidth = 2;
            this.canvasContext.stroke();
          })
        })
      }

      if( this.characterData ) {
        this.drawPigCharacter()
      }
    },

    movePipes() {
      if( this.pipes.length ) {
        let check = false;
        this.pipes.forEach((pipesPair, index) => {
          pipesPair.forEach(pipe => {
            pipe.x -= 1;
            if( index == 0 ) {
              if( pipe.x == 0 - this.initialGameData.pipeWidth ) check = true;
            }
          })
          if( check ) {
            this.pipes.splice(index, 1);
            check = false;
            this.generateNewPipe();
          }
        })
      }

      this.checkCollision();

      this.drawScreen();

      if( !this.gameOver ) {
        setTimeout(() => {
          this.movePipes();
        }, 20 / this.difficulty);
      }
    },

    checkCollision() {
      if( this.pipes.length ) {
        this.pipes.forEach(pipesPair => {
          let checkdOnce = false;
          pipesPair.forEach(pipe => {
            if( this.characterData.x + 30 >= pipe.x && this.characterData.x - 30 <= pipe.x + pipe.width ) {
              if( this.characterData.y + 30 >= pipe.y && this.characterData.y - 30 <= pipe.y + pipe.height ) {
                this.gameOver = true;
              }
            }
            if( this.characterData.x == pipe.x + pipe.width ) {
              checkdOnce = true;
            }
          })

          if( checkdOnce ) {
            this.points += 1;
          }
        })
      }
    },

    moveCharacter(position) {
      this.characterData.y = position[1]
      
      if( !this.firstPredictionDone ) {
        this.firstPredictionDone = true;
        this.$refs.counter.style.display = 'block';
        const counterNumbers = this.$refs.counter.querySelectorAll('span');
        console.log(counterNumbers[0]);
        counterNumbers[0].style.display = 'block';
        setTimeout(() => {
          counterNumbers[0].style.display = 'none';
          counterNumbers[1].style.display = 'block';
        }, 500);
        setTimeout(() => {
          counterNumbers[1].style.display = 'none';
          counterNumbers[2].style.display = 'block';
        }, 1000);
        setTimeout(() => {
          counterNumbers[2].style.display = 'none';
          counterNumbers[3].style.display = 'block';
        }, 1500);
        setTimeout(() => {
          counterNumbers[3].style.display = 'none';
          this.$refs.counter.style.display = 'none';
          if( !this.gameReady ) {
            this.movePipes()
            this.gameReady = true;
          }
        }, 2000);
      }
    }
  },
}
</script>

<style scoped>
#canvas {
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 2;
}
.points {
  position: fixed;
  top: 0;
  left: 0;
  text-transform: uppercase;
  color: green;
  font-size: 1.5rem;
  z-index: 3;
  padding: 10px;
  background: white;
  border: 1px solid black;
}
.rise-hand,
.counter,
.game-over {
  color: white;
  background: rgba(0, 0, 0, 0.5);
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  z-index: 4;
}
h1 {
  margin: 0;
}
.counter {
  display: none;
}
.counter span {
  font-size: 4rem;
  text-transform: uppercase;
}

.game-over h1 {
  font-size: 6rem;
  color: #ff0000;
}
</style>
