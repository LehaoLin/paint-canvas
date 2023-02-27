<template></template>

<script setup>
import { ref, onMounted } from "vue";
import * as PIXI from "pixi.js";

const props = defineProps({
  colors: Array,
});

onMounted(() => {
  const app = new PIXI.Application({
    width: 1210,
    height: 610,
    backgroundColor: 0xffffff,
  });

  const gridContainer = new PIXI.Container();
  gridContainer.position.set(5, 5);
  gridContainer.renderable = true;
  app.stage.addChild(gridContainer);

  const cellWidth = 20;
  const cellHeight = 20;
  const borderWidth = 1;
  const gridColor = 0xcccccc;
  const outerBorderColor = 0x000000; // set the color of the outer border separately

  // draw the outer border
  const outerBorder = new PIXI.Graphics();
  outerBorder.lineStyle(borderWidth * 2, outerBorderColor); // multiply the borderWidth by 2 to make it thicker
  outerBorder.drawRect(0, 0, 60 * cellWidth, 30 * cellHeight);
  gridContainer.addChild(outerBorder);

  for (let i = 0; i < 60; i++) {
    for (let j = 0; j < 30; j++) {
      const cell = new PIXI.Graphics();
      cell.lineStyle(borderWidth, 0xffffff);
      // cell.beginFill(0x000000);
      const colorIndex = i * 60 + j;
      try {
        cell.beginFill(props.colors[colorIndex]);
      } catch {
        cell.beginFill(0xffffff);
      }
      cell.drawRect(i * cellWidth, j * cellHeight, cellWidth, cellHeight);
      cell.endFill();
      gridContainer.addChild(cell);

      if (i === 0 || j === 0) {
        const line = new PIXI.Graphics();
        line.lineStyle(borderWidth, gridColor);
        if (i === 0) {
          line.moveTo(0, j * cellHeight);
          line.lineTo(60 * cellWidth, j * cellHeight);
        }
        if (j === 0) {
          line.moveTo(i * cellWidth, 0);
          line.lineTo(i * cellWidth, 30 * cellHeight);
        }
        gridContainer.addChild(line);
      }
    }
  }

  document.body.appendChild(app.view);
});
</script>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
