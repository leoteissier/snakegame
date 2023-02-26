<template>
  <div class="snake" :style="{ top: position.y + 'px', left: position.x + 'px' }">
    <!-- Le serpent -->
    <div v-for="part in body" :key="part.id" class="snake-part" :style="{ top: part.y + 'px', left: part.x + 'px' }"></div>
  </div>
</template>

<script>
export default {
  name: 'Snake',
  data() {
    return {
      position: {
        x: 125,
        y: 350,
      },
      body: [],
      isPaused: false,
    };
  },
  mounted() {
    window.addEventListener('keydown', (event) => {
      if (event.key === 'ArrowRight') {
        this.direction = 'right';
      } else if (event.key === 'ArrowLeft') {
        this.direction = 'left';
      } else if (event.key === 'ArrowUp') {
        this.direction = 'up';
      } else if (event.key === 'ArrowDown') {
        this.direction = 'down';
      }
    });
  },
  methods: {
    addBodyPart() {
      const lastPart = this.body[this.body.length - 1];

      this.body.push({
        id: this.body.length,
        x: lastPart.x,
        y: lastPart.y,
      });
    },
    move(x, y) {
      // Move the head of the snake
      this.position.x += x;
      this.position.y += y;

      // Move the rest of the body
      for (let i = this.body.length - 1; i > 0; i--) {
        this.body[i].x = this.body[i - 1].x;
        this.body[i].y = this.body[i - 1].y;
      }

      // Update the position of the first body part
      if (this.body.length > 0) {
        this.body[0].x = this.position.x;
        this.body[0].y = this.position.y;
      }
    },
    // move() {
    //   // Move the head of the snake
    //   if (this.direction === 'right') {
    //     this.position.x += 5;
    //   } else if (this.direction === 'left') {
    //     this.position.x -= 5;
    //   } else if (this.direction === 'up') {
    //     this.position.y -= 5;
    //   } else if (this.direction === 'down') {
    //     this.position.y += 5;
    //   }
    //
    //   // Move the rest of the body
    //   for (let i = this.body.length - 1; i > 0; i--) {
    //     this.body[i].x = this.body[i - 1].x;
    //     this.body[i].y = this.body[i - 1].y;
    //   }
    //
    //   // Update the position of the first body part
    //   if (this.body.length > 0) {
    //     this.body[0].x = this.position.x;
    //     this.body[0].y = this.position.y;
    //   }
    // },
  },
  created() {
    setInterval(() => {
      this.move();
    }, 100);
  },
};
</script>

<style scoped>

</style>