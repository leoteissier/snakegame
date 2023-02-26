<template>
  <div class="jeu">

    <div class="game-board" tabindex="0" @keydown="moveSnake">
      <!-- Le plateau de jeu -->
      <Snake ref="snake" />
      <div class="apple" :style="{ top: apple.y + 'px', left: apple.x + 'px' }"></div>

      <div v-if="gameOver" class="game-over-message">
        <p>GAME OVER!</p>
        <button @click="resetGame">start-again</button>
      </div>

      <div v-if="win" class="win-message">
        <p>WELL DONE!</p>
        <button @click="resetGame">play-again</button>
      </div>

      <button v-if="!playing" @click="startGame" class="start-button">start-game</button>
    </div>
    <div class="informations">
      <div class="rules">
        <p>// use keyboard</p>
        <p>// arrows to play</p>
        <p>Appuyez sur les flèches pour vous déplacer</p>
      </div>
      <div class="score">
        <p>// food left</p>
        <p>Score: {{ score }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import Snake from './components/Snake.vue';
export default {
  name: 'App',
  components: {
    Snake,
  },
  data() {
    return {
      apple: {
        x: 0,
        y: 0,
      },
      score: 0,
      gameOver: false,
      playing: false,
    };
  },
  mounted() {
    this.generateApple();
    this.checkCollisions();
  },
  methods: {
    moveSnake(event) {
      console.log(event.keyCode);
      const snake = this.$refs.snake;

      switch (event.keyCode) {
        case 37: // left
          snake.move(-10, 0);
          break;
        case 38: // up
          snake.move(0, -10);
          break;
        case 39: // right
          snake.move(10, 0);
          break;
        case 40: // down
          snake.move(0, 10);
          break;
      }

      snake.move();
      this.checkCollisions();

      // Vérifie si le serpent mange une pomme
      if (snake.position.x === this.apple.x && snake.position.y === this.apple.y) {
        snake.addBodyPart();
        this.generateApple();
      }
    },
    checkCollisions() {
      const gameBoard = this.$el;
      const snake = this.$refs.snake;

      if (!snake || !snake.$el) {
        return;
      }

      // Vérifie les collisions avec le bord gauche du plateau de jeu
      if (snake.$el.offsetLeft < 0) {
        snake.$el.style.left = '0px';
      }

      // Vérifie les collisions avec le bord supérieur du plateau de jeu
      if (snake.$el.offsetTop < 0) {
        snake.$el.style.top = '0px';
      }

      // Vérifie les collisions avec le bord droit du plateau de jeu
      if (snake.offsetLeft + snake.offsetWidth > gameBoard.offsetWidth) {
        snake.style.left = gameBoard.offsetWidth - snake.offsetWidth + 'px';
      }

      // Vérifie les collisions avec le bord inférieur du plateau de jeu
      if (snake.offsetTop + snake.offsetHeight > gameBoard.offsetHeight) {
        snake.style.top = gameBoard.offsetHeight - snake.offsetHeight + 'px';
      }
    },
    generateApple() {
      const gameBoard = this.$el;
      const snake = this.$refs.snake;

      if (!snake || !snake.$el) {
        return;
      }

      const apple = {
        x: Math.floor(Math.random() * (gameBoard.offsetWidth - 10)) + 10,
        y: Math.floor(Math.random() * (gameBoard.offsetHeight - 10)) + 10,
      };

      // Vérifie que la pomme n'apparaît pas sur le corps du serpent
      for (let i = 0; i < this.$refs.snake.body.length; i++) {
        const part = this.$refs.snake.body[i];

        if (part.x === apple.x && part.y === apple.y) {
          this.generateApple();
          return;
        }
      }
      // Positionne la pomme sur le plateau de jeu
      this.apple = apple;
    },
    startGame() {
      this.playing = true;

      // Add a body part every 5 seconds
      setInterval(() => {
        this.addBodyPart();
      }, 5000);
    },
  },
};
</script>

<style scoped>
p{
  font-family: "Fira Code", monospace;
}
.jeu{
  display: flex;
  width: 500px;
  height: 500px;
  padding: 20px;
  border-radius: 10px;
  background: linear-gradient(140deg, #175553, rgba(67,	217,	173, 0.13)) no-repeat;
}
.game-board {
  background-color: #011627;
  width: 250px;
  height: 100%;
  position: relative;
  margin: 0 auto;
  border-radius: 10px;
}

.apple {
  width: 10px;
  height: 10px;
  background-color: #43D9AD;
  box-shadow: #43D9AD 0 0 15px 0;
  position: absolute;
  border-radius: 50%;
}

.snake {
  position: absolute;
  width: 10px;
  height: 10px;
  background-color: #43D9AD;
  border-radius: 50%;
}

.snake-body {
  position: absolute;
  width: 10px;
  height: 10px;
  background-color: rgba(67, 217, 173, 0.5);
  border-radius: 50%;
}

.snake-body:last-child {
  background-color: white;
}

.start-button {
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  bottom: 5%;
  display: block;
  margin: 0 auto;
  background-color: #FEA55F;
  color: #01080E;
  padding: 5px 10px;
  border-radius: 5px;
  cursor: pointer;
}

.start-button:hover {
  background-color: #FFAC6B;
}

.informations{
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 250px;
  height: 100%;
}

.score {
  width: 80%;
  color: white;
  font-size: 14px;
  margin-bottom: 10px;
  padding: 10px;
  text-align: start;
}

.rules {
  width: 80%;
  color: white;
  background-color: rgba(1, 20, 35, 0.5);
  font-size: 14px;
  margin-bottom: 10px;
  padding: 10px;
  text-align: start;
}
</style>