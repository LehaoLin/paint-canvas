<template>
  <div id="container"></div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from "vue";

import Konva from "konva";

const props = defineProps({
  colors: Array,
});

const emit = defineEmits(["paint"]);

onMounted(() => {
  // create a stage with the specified width and height
  var stage = new Konva.Stage({
    container: "container",
    width: 1200,
    height: 600,
  });

  // define the color list
  const colorList = props.colors;
  console.log(colorList);
  var uu = 0;
  while (uu < 1800) {
    if (!colorList[uu]) {
      colorList.push("#ffffff");
    }
    uu += 1;
  }
  console.log(colorList);

  // create a layer to hold the grid
  var layer = new Konva.Layer();

  // calculate the size of each cell
  var cellWidth = stage.width() / 60;
  var cellHeight = stage.height() / 30;

  // loop through each row and column to create the cells
  for (var row = 0; row < 30; row++) {
    for (var col = 0; col < 60; col++) {
      // calculate the color index based on the row and column
      // var colorIndex = (row + col) % colorList.length;
      var colorIndex = row * 30 + col;
      var color = "";
      try {
        color = colorList[colorIndex];
      } catch {
        color = "#ffffff";
      }

      // create a rectangle shape for each cell with the specified color
      var rect = new Konva.Rect({
        x: col * cellWidth,
        y: row * cellHeight,
        width: cellWidth,
        height: cellHeight,
        fill: color,
        stroke: "black",
        strokeWidth: 1,
      });

      rect.on("click", function () {
        let col_index = (this.attrs.x + 20) / 20;
        let row_index = (this.attrs.y + 20) / 20;
        // console.log("Cell clicked:", col_index, row_index);
        let temp_color = colorList[(row_index - 1) * 30 + col_index - 1];
        // console.log("color", temp_color);
        if (temp_color == "#ffffff") {
          emit("paint", { col_index, row_index, status: "available" });
        } else {
          emit("paint", { col_index, row_index, status: "unavailable" });
        }
      });

      // rect.on("mouseover", function () {
      //   this.fill("gray");
      //   console.log("okk");
      //   layer.draw();
      // });

      // rect.on("mouseout", function () {
      //   this.fill("#ffffff");
      //   layer.draw();
      // });

      // add the rectangle to the layer
      layer.add(rect);
    }
  }

  // add the layer to the stage
  stage.add(layer);
});
</script>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
