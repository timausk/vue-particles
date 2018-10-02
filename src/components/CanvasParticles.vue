<template>
  <div class="particle-canvas-wrapper">
    <canvas ref="particle-canvas"></canvas>
  </div>
</template>

<script>
export default {
  data() {
    return {
      canvas: {
        height: 0,
        width:  0
      },
      settings: {
        particle: 250,
        velocity: 1,
        color: '#FFA237',
      }
    }
  },
  mounted() {
    this.$nextTick(() => {
      window.addEventListener('resize', () => {
        this.canvas.height = this.getWindowHeight();
        this.canvas.width  = this.getWindowWidth();
        this.resizeCanvas();
      });
    });
    this.initializeParticles();
  },
  methods: {
    initializeParticles() {
      // get width and height
      let width  = this.getWindowWidth();
      let height = this.getWindowHeight();

      // setup canvas size
      const canvas  = this.$refs['particle-canvas'];
      const context = canvas.getContext('2d');
      canvas.width  = width;
      canvas.height = height;
      context.fillStyle = this.settings.color;

      // get random values
      const getRandomValue = value => Math.random() * value;

      const getRandomVelocity = () => {
        if (Math.random() < 0.5) {
          return Number(- +Math.random() * this.settings.velocity);
        }
        return Math.random() * this.settings.velocity;
      };

      // create circles
      const circle = [];
      for(let i = 0; i < this.settings.particle; i += 1) {
        circle[i] = {
          posX: getRandomValue(width),
          posY: getRandomValue(height),
          velX: getRandomVelocity(),
          velY: getRandomVelocity(),
          size: getRandomValue(1),
          alpha: getRandomValue(0.75),
        };
      }

      const animate = () => {
        context.clearRect(0, 0, canvas.width, canvas.height);
        for (let i = 0; i < circle.length; i += 1) {
          context.beginPath();
          context.arc(
            (circle[i].posX += circle[i].velX),
            (circle[i].posY += circle[i].velY),
            circle[i].size, 0, Math.PI * 2, true,
          );
          context.closePath();
          context.globalAlpha = circle[i].alpha;
          context.fill();
          if (circle[i].posX <= -1) {
            circle[i].posX = -1;
            circle[i].posY = getRandomValue(height);
            circle[i].velX = Math.random() * this.settings.velocity;
            circle[i].velY = getRandomVelocity();
          }
          if (circle[i].posY <= -1) {
            circle[i].posX = getRandomValue(width);
            circle[i].posY = -1;
            circle[i].velX = getRandomVelocity();
            circle[i].velY = Math.random() * this.settings.velocity;
          }
          if (circle[i].posX >= width + 1) {
            circle[i].posX = width + 1;
            circle[i].posY = getRandomValue(height);
            circle[i].velX = Number(- +Math.random() * this.settings.velocity);
            circle[i].velY = getRandomVelocity();
          }
          if (circle[i].posY >= height + 1) {
            circle[i].posX = getRandomValue(width);
            circle[i].posY = height + 1;
            circle[i].velX = getRandomVelocity();
            circle[i].velY = Number(- +Math.random() * this.settings.velocity);
          }
        }
        requestAnimationFrame(animate);
      };
      requestAnimationFrame(animate);
    },
    resizeCanvas() {
      let canvas  = this.$refs['particle-canvas'];
      let context = canvas.getContext('2d');
      canvas.width  = this.getWindowWidth();
      canvas.height = this.getWindowHeight();
      context.fillStyle = this.settings.color;
    },
    getWindowHeight() {
      return window.innerHeight || document.documentElement.clientHeight || document.getElementsByTagName('body')[0].clientHeight;
    },
    getWindowWidth() {
      return window.innerWidth || document.documentElement.clientWidth|| document.getElementsByTagName('body')[0].clientWidth;
    }
  }
}
</script>
