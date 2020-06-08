<template>
    <div id="app">
        <DataEmitter msg="Emitter" v-bind:frequency="100"/>
        <LiveGraph msg="Fast Graph Test" dataKey="point1" />
        <LiveGraph msg="Slow Graph Test" dataKey="point2" />
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
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}
</style>
