
<template>
  <div>
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
  </div>
</template>

<script setup>
import { ref } from "vue";
import DragStage from "./components/DragStage.vue";
const lines = ref([]);
const currentLine = ref(-1);
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
</style>
