<template>
    <div class="container">
        <div v-if="true">
            <el-row>
                <el-col :span="2">time</el-col>
                <el-col :span="12"><el-slider v-model="time" show-input/></el-col>
            </el-row>
            <el-row>
                <el-col :span="2">x</el-col>
                <el-col :span="12"><el-slider v-model="field" show-input/></el-col>
            </el-row>
            <el-button @click="$emit('clickDelete', 'selected node')">Delete</el-button>
        </div>
        <div v-else-if="true">
            <el-row>
                <el-col :span="2">ease</el-col>
                <el-col :span="12">
                    <el-select v-model="ease">
                        <el-option
                                v-for="item in easeOptions"
                                :key="item"
                                :label="item"
                                :value="item">
                        </el-option>
                    </el-select>
                </el-col>
            </el-row>
        </div>
    </div>
</template>

<script lang="ts">
    import {Component, Prop, Vue} from 'vue-property-decorator';

    @Component
    export default class Tool extends Vue {
        @Prop() private selectedObject:fabric.Object;

        private time:number = 0;
        private field:number = 0;
        private ease:string = "linear";
        private easeOptions:string[] = [
            "linear",
            "quadIn",
            "quadOut"
        ];
        private nodeField = [{
            name: "time",
            value: 0
        }, {
            name: "x",
            value: 0
        }, {
            name: "ease",
            value: 0
        }];
    };
</script>

<style scoped>
    .container {
        padding: 32px;
    }

    .el-row {
        margin: 16px;
    }
    
    .el-slider {
        color: #753;
    }
</style>