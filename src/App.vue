<template>
    <div id="app">
        <div class="sidebar">
            <DataEmitter msg="Emitter" v-bind:frequency="100"/>
        </div>
        <div class="main" style="grid-auto-flow: row dense;">
            <LiveGraph msg="Fast Graph" dataKey="point1" />
            <LiveGraph msg="Slow Graph" dataKey="point2" />
            <LiveGraph msg="Wide Graph" dataKey="point1" class="wide" />
        </div>
    </div>
</template>

<script>
import LiveGraph from './components/LiveGraph.vue'
import DataEmitter from './components/DataEmitter.vue'
import Rickshaw from "rickshaw"
import Vue from 'vue'

Object.defineProperty(Vue.prototype, '$rickshaw', { value: Rickshaw });

export default {
    name: 'App',
    components: {
        DataEmitter,
        LiveGraph
    }
}
</script>

<style>

body {
    background-color: #1d2325;
}

#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #d4d4d4;
    padding-right: 30px;
}

.main {
    margin-left: 300px;
    font-size: 22px;
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 30px;
    padding: 30px;
    text-align: center;
}

.wide {
    grid-column: auto / span 2;
}

.sidebar {
    border-right: 2px solid #5c6370;
    border-left: 2px solid #5c6370;
    height: 100%;
    width: 300px;
    position: fixed;
    z-index: 1;
    top: 0;
    left: 0;
    background-color: #242b2e;
    overflow-x: hidden;
    padding-top: 50px;
}


</style>
