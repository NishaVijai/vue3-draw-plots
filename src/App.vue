<template>
  <h1>
    Draw editable plots - using mouse click on the gray background area to draw
    plots
  </h1>
  <section class="button-container">
    <button @click="undoPlotPointHandle" v-if="points.length > 0">
      Undo point
    </button>
    <button @click="redoPlotPointHandle" v-if="removedPlotPoints.length > 0">
      Redo point
    </button>
  </section>
  <section @click="plotPointHandle" class="section-container">
    <p
      class="point"
      v-for="point in points"
      :key="point"
      :style="{ top: `${point.y}%`, left: `${point.x}%` }"
    ></p>
  </section>
</template>

<script setup>
import { ref } from "vue";
const points = ref([]);
const removedPlotPoints = ref([]);

const plotPointHandle = (e) => {
  if (points.value === undefined) return;
  points.value.push({
    x: (e.offsetX / window.innerWidth) * 100,
    y: (e.offsetY / window.innerHeight) * 100,
  });
};

const undoPlotPointHandle = () => {
  const removedCurrentPlotPoint = points.value.pop();
  removedPlotPoints.value.push(removedCurrentPlotPoint);
};

const redoPlotPointHandle = () => {
  const removedPreviousPlotPoint = removedPlotPoints.value.pop();
  points.value.push(removedPreviousPlotPoint);
};
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

html {
  height: 100%;
}

body {
  font-size: 16px;
  height: 100%;
  overflow-x: hidden;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.section-container {
  background-color: lightgrey;
  border: 2px solid red;
  height: 100vh;
  width: 100vw;
  cursor: pointer;
  position: relative;
}

.point {
  background-color: #fff;
  border-radius: 50%;
  height: 40px;
  width: 40px;
  position: absolute;
  pointer-events: none;
}

.button-container {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  flex-wrap: wrap;
}

button {
  background-color: tan;
  font-size: 1.2rem;
  height: 5vh;
  width: 20vw;
}
</style>
