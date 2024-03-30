<script>
  import { onMount } from 'svelte';
  let canvas;
  let ctx;
  let isDrawing = false;
  let color = '#000000';
  let lineWidth = 3;
  let isErasing = false;
  onMount(() => {
    ctx = canvas.getContext('2d');
    ctx.fillStyle = "white";
    ctx.fillRect(0, 0, canvas.width, canvas.height);
    const draw = (e) => {
      if (!isDrawing) return;
      ctx.strokeStyle = isErasing ? 'white' : color;
      ctx.lineWidth = lineWidth;
      ctx.lineTo(e.offsetX, e.offsetY);
      ctx.stroke();
    };
    const startDrawing = (e) => {
      isDrawing = true;
      ctx.beginPath();
      ctx.moveTo(e.offsetX, e.offsetY);
    };
    const stopDrawing = () => {
      isDrawing = false;
      ctx.closePath();
    };
    canvas.addEventListener('mousedown', startDrawing);
    canvas.addEventListener('mousemove', draw);
    canvas.addEventListener('mouseup', stopDrawing);
    canvas.addEventListener('mouseleave', stopDrawing);
    return () => {
      canvas.removeEventListener('mousedown', startDrawing);
      canvas.removeEventListener('mousemove', draw);
      canvas.removeEventListener('mouseup', stopDrawing);
      canvas.removeEventListener('mouseleave', stopDrawing);
    };
  });
  const clearCanvas = () => {
    ctx.fillStyle = "white";
    ctx.fillRect(0, 0, canvas.width, canvas.height);
  };
  const toggleEraser = () => {
    isErasing = !isErasing;
  };
</script>

<div class="drawing-app">
  <div class="controls">
    <input type="color" bind:value={color} disabled={isErasing} />
    <input type="range" min="1" max="50" bind:value={lineWidth} />
    <button on:click={toggleEraser}>{isErasing ? 'Draw' : 'Erase'}</button>
    <button on:click={clearCanvas}>Clear Canvas</button>
  </div>
  <canvas bind:this={canvas} width="800" height="600"></canvas>
</div>

<style>
  .drawing-app {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  canvas {
    border: 2px solid #000000;
  }
  .controls {
    margin-bottom: 20px;
  }
</style>