<template>
    <div>
        <div class="header">Export</div>
        <div class="textContainer">
            <el-input
                    type="textarea"
                    :rows="8"
                    v-model="output"
                    resize="none"
                    readonly>
            </el-input>
        </div>
        <el-button plain size="medium" @click="">Export</el-button>
    </div>
</template>

<script lang="ts">
    import {Component, Vue} from 'vue-property-decorator';

    @Component
    export default class Tool extends Vue {
        private output:string = `{
    "initialValue": 10,
    "tweens": [{
        "time": 0.7,
        "value": 40,
        "ease": "quadOut"
    }, {
        "time": 0.8,
        "value": -40,
        "ease": "cubicIn"
    }, {
        "time": 1.2,
        "value": 10,
        "ease": "quadOut"
    }]
}`;
    };
</script>

<style scoped>
    div {
        height: 100%;
        background: #555;
        text-align: left;
    }

    .header {
        padding: 8px 0px;
        display: block;
        width: 100%;
        background: #333;
        text-align: center;
    }

    .textContainer {
        margin: 16px;
    }

    .el-button {
        margin-left: 16px;
    }
</style>