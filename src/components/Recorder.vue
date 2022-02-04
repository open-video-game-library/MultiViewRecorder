<template>
    <div class="wrapper">
        <CameraSelect />
        <CameraSelect />
        <canvas :width="canvasWidth" :height="canvasHeight">Canvas not available.</canvas>
        <CameraSelect />
        <CameraSelect />
    </div>
</template>

<script>
import CameraSelect from './CameraSelect.vue'

export default {
    name: 'Recorder',
    components: {
        CameraSelect
    },
    props: {
        canvasHeight: Number
    },
    watch: {
        canvasHeight() {
            this.canvasWidth = (this.canvasHeight / 9) * 16;
            this.selectWidth = (this.canvasWidth / 2) + 'px';
            this.canvas_height = this.canvasHeight + 'px';
        }
    },
    data() {
        return {
            canvasWidth: 640,
            selectWidth: '320px',
            canvas_height: '360px',
        }
    }
}
</script>

<style scoped>
.wrapper {
    width: 1280px;
    height: 720px;
    display: grid;
    grid-template-columns: v-bind(selectWidth) v-bind(selectWidth);
    grid-template-rows: 26px v-bind(canvas_height) 26px;
    /* grid-template-columns: 50% 50%;
    grid-template-rows: 26px 100% 26px; */
}
canvas {
    grid-column: 1 / 3;
}
</style>
