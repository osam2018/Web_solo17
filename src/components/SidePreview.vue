<template>
    <div>
        <canvas id="previewCanvas" width="399" height="399"></canvas>
    </div>
</template>

<script lang="ts">
    import {Component, Vue} from 'vue-property-decorator';
    import {fabric} from 'fabric';

    @Component
    export default class Tool extends Vue {
        private canvas!:fabric.StaticCanvas;

        private actor!:fabric.Rect;
        mounted () {
            this.canvas = new fabric.StaticCanvas("previewCanvas");

            this.actor = new fabric.Rect({
                width: 30,
                height: 30,
                fill: "#678edb",
                // stroke: "#285974",
                // strokeWidth: 3
            });
            this.canvas.add(this.actor);
            this.canvas.centerObject(this.actor);
        }
    };
</script>

<style scoped>
    div {
        height: 400px;
        background: #555;
        border-bottom: 1px solid #777;
    }
</style>