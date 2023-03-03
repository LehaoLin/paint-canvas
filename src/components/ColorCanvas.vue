<template>
  <div id="container"></div>
</template>

<script setup>
import { ref, onMounted, onUnmounted, computed } from "vue";

import Konva from "konva";

const props = defineProps({
  colors: Array,
});

const emit = defineEmits(["paint"]);

const stage = ref();
const layer = ref();

onMounted(() => {
  // create a stage with the specified width and height
  Konva.autoDrawEnabled = false;
  stage.value = new Konva.Stage({
    container: "container",
    width: 1200,
    height: 600,
  });

  // define the color list
  const colorList = props.colors;
  // console.log(colorList);

  var uu = 0;
  while (uu < 1800) {
    if (!colorList[uu]) {
      colorList.push("#ffffff");
    }
    uu += 1;
  }

  // create a layer to hold the grid
  layer.value = new Konva.Layer();

  // calculate the size of each cell
  var cellWidth = stage.value.width() / 60;
  var cellHeight = stage.value.height() / 30;

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

      rect.on("mouseover", function (evt) {
        var box = evt.target;
        box.fill("gray");
        document.body.style.cursor = "pointer";
        box.draw();
      });

      rect.on("mouseout", function (evt) {
        var box = evt.target;
        box.fill("#ffffff");
        document.body.style.cursor = "default";
        box.draw();
      });

      // add the rectangle to the layer
      layer.value.add(rect);
    }
  }

  // add the layer to the stage
  stage.value.add(layer.value);
});
</script>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
