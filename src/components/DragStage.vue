<template>
    <div
        class="drag-stage"
        :class="{ current: props.current }"
    >
        <svg v-if="positions.length > 1">
            <path :d="positions.map((item, index) => `${index === 0 ? 'M' : 'L'} ${item.x} ${item.y}`).join(' ')" />
        </svg>
        <DragPod
            v-for="(pod, i) in positions"
            :key="i"
            :index="i"
            @on-update="onUpdate"
        />
        <button
            v-if="current"
            class="add-pod-button"
            @click="addPod"
        >
            Add Pod
        </button>
        <p>{{}}</p>
    </div>
</template>
<script setup>
import { ref, computed } from 'vue';
import DragPod from './DragPod.vue';
const props = defineProps({
    index: {
        required: true,
        type: Number,
    },
    current: {
        required: true,
        type: Boolean,
    }
});
const emit = defineEmits(('line-update'));
const positions = ref([]);
const addPod = () => {
    positions.value.push({x: 0, y: 0});
    emit('line-update', { index: props.index, positions });
};
const onUpdate = ({x, y, index}) => {
    positions.value[index].x = x;
    positions.value[index].y = y;
    emit('line-update', { index: props.index, positions });
}
const pathString = computed(() => positions.map((item, index) => `${index === 0 ? 'M' : 'L'} ${item.x} ${item.y}`).join(' '));
</script>

<style>
.drag-stage{
    position: fixed;
    left: 0;
    top: 0;
    width: 500px;
    height: 500px;
    border: 1px solid #cccccc;
    pointer-events: none;
    opacity: .5;
}
.drag-stage.current{
    pointer-events: all;
    opacity: 1;
}
.drag-stage svg{
    position: absolute;
    pointer-events: none;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
}
.drag-stage svg{
    fill: transparent;
    stroke: #000000;
    stroke-width: 1px;
}
.drag-stage .add-pod-button{
    position: fixed;
    top: 0;
    right: 120px;
}
</style>
