
<template>
  <div>
    <div
      class="bg-container"
      :style="{ 'background-position': 'center', 'background-size': 'contain', 'background-repeat': 'no-repeat', 'background-image': `url(${bgImage})` }"
    />
    <DragStage
      v-for="(line, i) in lines"
      :key="i"
      :index="i"
      :current="i === currentLine"
      @line-update="lineUpdate"
    />
    <div class="line-list">
      <button
        v-for="(line, i) in lines"
        :key="i"
        @click="currentLine = i"
      >
        {{ i }}
      </button>
      <button @click="addLine">Add Line</button>
    </div>
    <input
      type="text"
      v-model="bgImage"
      :style="{ position: 'fixed', 'bottom': 0, 'left': 0 }"
    />
  </div>
</template>

<script setup>
import { ref } from "vue";
import DragStage from "./components/DragStage.vue";
const lines = ref([]);
const currentLine = ref(-1);
const bgImage = ref('');
const addLine = () => {
  lines.value.push([]);
  currentLine.value = lines.value.length - 1;
}
const lineUpdate = ({ index, positions }) => {
  lines.value[index] = positions;
}
</script>

<style scoped>
.line-list{
  position: fixed;
  top: 0;
  right: 0;
  display: flex;
  flex-direction: column;
}
.bg-container{
    position: fixed;
    left: 0;
    top: 0;
    width: 500px;
    height: 500px;
    border: 1px solid #cccccc;
    pointer-events: none;
    opacity: .5;
}
</style>
