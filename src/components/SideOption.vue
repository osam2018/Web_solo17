<template>
    <div>
        <div class="header">{{selector}}</div>
        <el-radio v-for="field in data" v-model="selector" :label="field">{{field}}</el-radio>
    </div>
</template>

<script lang="ts">
    import {Component, Vue} from 'vue-property-decorator';

    @Component
    export default class Tool extends Vue {
        private data:string[] = ["x", "y", "angle"];
        private selector:string = "Setting";
    };
</script>

<style scoped>
    div {
        height: 200px;
        background: #555;
        border-bottom: 1px solid #666;
    }

    .header {
        padding: 8px 0px;
        display: block;
        width: 100%;
        height: auto;
        background: #333;
    }

    .el-radio {
        margin: 8px;
        padding: 8px;
        display: block;
        text-align: left;
        color: #222;
    }

    .el-radio + .el-radio {
        margin: 8px;
    }
</style>