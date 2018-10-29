<template>
    <el-container>
        <el-header class="Header" height="60px">TweenEdit</el-header>
        <Tool/>
    </el-container>
</template>

<script lang="ts">
    import {Component, Prop, Vue} from 'vue-property-decorator';
    import Tool from './components/Tool.vue';

    @Component({
        components: {
            Tool
        },
    })

    export default class App extends Vue {
    }
</script>

<style>
    body {
        margin: 0px;
        height: 100%;
        font-family: Arial, sans-serif;
        font-size: large;
    }

    .Header {
        background-color: #333;
        border-bottom: #666 solid 1px;

        color: #aaaaaa;
        text-align: left;
        line-height: 60px;
        font-weight: bolder;
        font-family: "Segoe UI";
        font-size: 24px;
        text-shadow: 2px 2px 2px #222;
    }
</style>
