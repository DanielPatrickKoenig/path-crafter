<template>
    <div
        class="drag-pod"
        :style="{ left: `${dragData.x}px`, top: `${dragData.y}px` }"
        @mousedown="down"
    >
        <div class="inner-pod">
            <span>{{ props.index }}</span>
        </div>
        <div
            v-if="dragData.dragging"
            class="drag-overlay"
            @mousemove="move"
            @mouseup="up"
        />

    </div>
</template>

<script setup>
import { reactive, onMounted } from 'vue';
const props = defineProps({
    index: {
        required: true,
        type: Number,
    },
}); 
const emit = defineEmits('on-update');
const dragData = reactive({
    x: 0,
    y: 0,
    dragging: false,
});
const down = (e) => {
    dragData.dragging = true;
}

const move = (e) => {
    if (dragData.dragging) {
        dragData.x = e.clientX;
        dragData.y = e.clientY;
        emit('on-update', { index: props.index, x: dragData.x, y: dragData.y });
    }
}
const up = () => {
    dragData.dragging = false;
}
onMounted(() => {
    emit('on-update', { index: props.index, x: dragData.x, y: dragData.y });
})
</script>
<style>
.drag-pod{
    width: 0;
    height: 0;
    position: absolute;
    z-index: 2000;
}
.drag-pod .inner-pod{
    width: 20px;
    height: 20px;
    background: #cc0000;
    margin-left: -10px;
    margin-top: -10px;
    color: white;
}
.drag-pod .inner-pod span{
    pointer-events: none;
}
.drag-pod .drag-overlay{
    position: fixed;
    top: 0;
    left: 0;
    width: 500px;
    height: 500px;
}
</style>
