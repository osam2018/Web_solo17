<template>
    <el-container style="height: 900px">
        <el-main>
            <el-container style="height: 100%">
                <el-main>
                    <GraphEditor
                            ref="graphEditor"
                            :width="width"
                            :height="height"
                            @selectionCreated="onSelectionCreated($event)"/>
                </el-main>
                <el-footer height="250px" style="background: #333;">
                    <GraphPropertyEditor
                            :selectedObject="selectedObject"
                            ref="graphPropertyEditor"
                            />
                </el-footer>
            </el-container>
        </el-main>
        <el-aside width="400px">
            <el-container direction="vertical">
                <SidePreview/>
                <SideOption/>
                <SideExport/>
            </el-container>
        </el-aside>
    </el-container>
</template>

<script lang="ts">
    import {Component, Vue} from 'vue-property-decorator';
    import GraphEditor from "./GraphEditor.vue";
    import GraphPropertyEditor from "./GraphPropertyEditor.vue";
    import SidePreview from './SidePreview.vue';
    import SideOption from './SideOption.vue';
    import SideExport from "./SideExport.vue";
    import {fabric} from "fabric";

    @Component({
        components: {
            SideExport,
            GraphPropertyEditor,
            GraphEditor,
            SidePreview,
            SideOption
        }
    })
    export default class Tool extends Vue {
        private width:number = 1200;
        private height:number = 600;

        private selectedObject:fabric.Object;
        private onSelectionCreated(e) {
            this.selectedObject = e.target;
        }
    };
</script>

<style scoped>
    .el-container {
        padding: 0px;
    }

    .el-aside {
        background-color: #555;
        color: #888;
        text-align: center;

        border-left: 1px solid #666;
    }

    .el-main {
        background-color: #555;
        color: #888;
        text-align: center;
        padding: 0px;
    }

    .el-footer {
        background: #333;
        border-top: 1px solid #666;
    }
</style>