<template>
  <canvas ref="sakuraCanvas"></canvas>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from "vue";

// Canvas reference
const sakuraCanvas = ref(null);
let ctx, animationFrame;
const sakuraLeaves = [];

// Sakura Leaf Class
class Sakura {
  constructor(canvasWidth, canvasHeight) {
    this.canvasWidth = canvasWidth;
    this.canvasHeight = canvasHeight;
    this.x = Math.random() * this.canvasWidth;
    this.y = Math.random() * -this.canvasHeight;
    this.size = Math.random() * 5 + 10;
    this.speed = Math.random() * 2 + 1;
    this.angle = Math.random() * 360;
    this.spinSpeed = Math.random() * 2 - 1;
    this.swing = Math.random() * 1 + 2; // Horizontal swinging
  }

  update() {
    this.y += this.speed;
    this.x += Math.sin((this.angle * Math.PI) / 180) * this.swing;
    this.angle += this.spinSpeed;

    if (this.y > this.canvasHeight) {
      this.y = Math.random() * -this.canvasHeight;
      this.x = Math.random() * this.canvasWidth;
    }
  }

  draw(ctx) {
    ctx.save();
    ctx.translate(this.x, this.y);
    ctx.rotate((this.angle * Math.PI) / 180);
    drawSakuraLeaf(ctx, this.size);
    ctx.restore();
  }
}

// Function to Draw Sakura Petals
const drawSakuraLeaf = (ctx, size) => {
  ctx.beginPath();
  ctx.moveTo(0, -size / 2);
  ctx.bezierCurveTo(size / 2, -size, size, size / 2, 0, size);
  ctx.bezierCurveTo(-size, size / 2, -size / 2, -size, 0, -size / 2);
  ctx.closePath();

  const gradient = ctx.createLinearGradient(-size / 2, -size / 2, size / 2, size / 2);
  gradient.addColorStop(0, "#ffb6c1");
  gradient.addColorStop(1, "#ff69b4");
  ctx.fillStyle = gradient;
  ctx.fill();

  ctx.strokeStyle = "#ff4f85";
  ctx.lineWidth = 0.5;
  ctx.stroke();
};

// Initialize Canvas
const initCanvas = () => {
  const canvas = sakuraCanvas.value;
  ctx = canvas.getContext("2d");

  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;

  for (let i = 0; i < 50; i++) {
    sakuraLeaves.push(new Sakura(canvas.width, canvas.height));
  }

  animateSakura();
};

// Resize Canvas
const resizeCanvas = () => {
  const canvas = sakuraCanvas.value;
  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;
};

// Sakura Animation
const animateSakura = () => {
  ctx.clearRect(0, 0, sakuraCanvas.value.width, sakuraCanvas.value.height);

  sakuraLeaves.forEach((leaf) => {
    leaf.update();
    leaf.draw(ctx);
  });

  animationFrame = requestAnimationFrame(animateSakura);
};

// Lifecycle Hooks
onMounted(() => {
  window.addEventListener("resize", resizeCanvas);
  initCanvas();
});

onBeforeUnmount(() => {
  window.removeEventListener("resize", resizeCanvas);
  cancelAnimationFrame(animationFrame);
});
</script>

<style scoped>
canvas {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none; /* This prevents the canvas from interfering with clicks and interaction */
  z-index: -1; /* Ensure the canvas stays behind other content */
}
</style>
