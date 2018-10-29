<template>
    <div>
        <canvas id="GraphCanvas" :width="width" :height="height"></canvas>
        <el-button @click="onClickAddNode()">Add node</el-button>
    </div>
</template>

<script lang="ts">
    import {Component, Prop, Vue} from 'vue-property-decorator';
    import {fabric} from 'fabric';

    @Component
    export default class Tool extends Vue {
        @Prop() private width!:number;
        @Prop() private height!:number;

        private padding:number = 50;
        private canvas!:fabric.Canvas;
        private nodes:fabric.Circle[] = [];
        private edges:fabric.Path[] = [];

        mounted() {
            this.canvas = new fabric.Canvas("GraphCanvas", {
                selection: false,
                targetFindTolerance: 10
            });

            this.canvas.on({
                "object:moving": this.onObjectMoving,
                "selection:created": (e) => this.$emit("selectionCreated", e)
            });

            let start = this.insertNode(this.padding, this.height / 2, 0);
            start.name = "start";
            start.lockMovementX = true;

            let end = this.insertNode(this.width - this.padding, this.height / 2, 1);
            end.name = "end";
            end.lockMovementX = true;

            this.insertEdge(0);
        }

        private onClickAddNode() {
            let object = this.canvas.getActiveObject();
            let otherNode:fabric.Circle;
            let targetIndex:number;

            if (!object || object.name === "end") {
                targetIndex = this.nodes.length - 1;
                object = this.nodes[targetIndex];
                otherNode = this.nodes[targetIndex - 1];
            } else if (object instanceof fabric.Circle) {
                targetIndex = this.nodes.indexOf(object as fabric.Circle) + 1;
                otherNode = this.nodes[targetIndex];
            } else if (object instanceof fabric.Path) {
                targetIndex = this.edges.indexOf(object as fabric.Path) + 1;
                otherNode = this.nodes[targetIndex];
            }

            this.insertNode(
                (object.left + otherNode.left) / 2,
                (object.top + otherNode.top) / 2,
                targetIndex
            );
            this.removeEdge(targetIndex - 1);
            this.insertEdge(targetIndex - 1);
            this.insertEdge(targetIndex);
        }

        private onObjectMoving(e:fabric.IEvent) {
            let object = e.target as fabric.Object;

            if (object instanceof fabric.Circle) {
                if (object.top < this.padding) {
                    object.set({top: this.padding});
                } else if (object.top > this.height - this.padding) {
                    object.set({top: this.height - this.padding});
                }

                if (object.name !== "start" && object.name !== "end") {
                    const index = this.nodes.indexOf(object);

                    if (object.left < this.nodes[index - 1].left) {
                        object.set({left: this.nodes[index - 1].left});
                    } else if (object.left > this.nodes[index + 1].left) {
                        object.set({left: this.nodes[index + 1].left});
                    }
                }
            }
        }

        // private onSelectionCreated(e:fabric.IEvent) {
        //
        //     if (object instanceof fabric.Circle) {
        //         object.set({
        //             stroke: "#507fde",
        //             fill: "#27406e"
        //         });
        //     } else if (object instanceof fabric.Path) {
        //         object.set({
        //             stroke: "#507fde"
        //         });
        //     }
        // }
        //
        // private onSelectionCleared(e:fabric.IEvent) {
        //     let object = e.target;
        //
        //     if (object instanceof fabric.Circle) {
        //         object.set({
        //             stroke: "#deaf4e",
        //             fill: "#6f5727"
        //         });
        //     } else if (object instanceof fabric.Circle) {
        //         object.set({
        //             stroke: "#deaf4e"
        //         });
        //     }
        // }

        private createNode(x:number, y:number):fabric.Circle {
            return new fabric.Circle({
                left: x,
                top: y,
                radius: 8,
                fill: "#6f5727",
                stroke: "#deaf4e",
                strokeWidth: 3,
                // hasBorders: false,
                hasRotatingPoint: false,
                hasControls: false,
                originX: "center",
                originY: "center"
            });
        }

        private createEdge(svgPath:string):fabric.Path {
            return new fabric.Path(svgPath, {
                stroke: "#deaf4e",
                strokeWidth: 3,
                // hasBorders: false,
                hasRotatingPoint: false,
                hasControls: false,
                lockMovementX: true,
                lockMovementY: true,
                perPixelTargetFind: true
            });
        }

        private insertEdge(nodeIndex:number):fabric.Path {
            const node = this.nodes[nodeIndex];
            const otherNode = this.nodes[nodeIndex + 1];

            const edge = this.createEdge(`M ${node.left} ${node.top} L ${otherNode.left} ${otherNode.top}`);
            this.canvas.insertAt(edge, 0, false);
            this.edges.splice(nodeIndex, 0, edge);
            return edge;
        }

        private removeEdge(nodeIndex:number) {
            this.canvas.remove(this.edges[nodeIndex]);
            this.edges.splice(nodeIndex);
        }

        public insertNode(x:number, y:number, index:number):fabric.Circle {
            const node = this.createNode(x, y);
            // this.canvas.insertAt(node, index, false);
            this.canvas.add(node);
            this.nodes.splice(index, 0, node);

            return node;
        }

        public removeNode(index:number) {
            this.canvas.remove(this.nodes[index]);
            this.nodes.splice(index);
        }
    };
</script>

<style scoped>
    div {
        text-align: center;
    }

    canvas {
        display: block;
        height: 100%;
        margin: auto auto;
    }
</style>