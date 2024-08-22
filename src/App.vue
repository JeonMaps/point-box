<template>
  <h1>Point Box</h1>
  <div class="buttons">
    <button @click="undoPoint">Undo Point</button>
    <button @click="redoPoint">Redo Point</button>
  </div>
  <div class="point-box" @click="plotPoint">
    <div v-for="(point, index) in points" class="point" :key="index"
      :style="{ top: `${point.y}%`, left: `${point.x}%` }">
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const points = ref([])
const deletedPoints = ref([])

const pointExists = (point, array) => {
  return array.some(p => p.x === point.x && p.y === point.y);
};

const plotPoint = (e) => {
  const newPoint = {
    x: e.offsetX / window.innerWidth * 100,
    y: e.offsetY / window.innerHeight * 100,
  }

  if (!pointExists(newPoint, points.value) && !pointExists(newPoint, deletedPoints.value)) {
    points.value.push(newPoint);
  }
}

const undoPoint = () => {
  if (points.value.length === 0) return;
  const deletedPoint = points.value.pop();
  if (deletedPoint) {
    deletedPoints.value.push(deletedPoint);
  }
};

const redoPoint = () => {
  if (deletedPoints.value.length === 0) return;
  const reAddedPoint = deletedPoints.value.pop();
  if (reAddedPoint) {
    points.value.push(reAddedPoint);
  } else {
    console.error('reAddedPoint is undefined');
  }
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.point-box {
  height: 100vh;
  background-color: gray;
  position: relative;
}

.point {
  width: 10px;
  height: 10px;
  background-color: aqua;
  position: absolute;
  border-radius: 50%;
}
</style>
