<template>
    <div class="hello">
        <h1>{{ msg }}</h1>
        <div id="chart_container">
        <div id="chart" class="rickshaw-graph"></div>
        </div>
    </div>
</template>

<script>
    import Rickshaw from "rickshaw"
    export default {
        name: 'HelloWorld',
        props: {
            msg: String
        },
        mounted() {
            this.initChart();
        },
        methods: {
            initChart() {
                var graph = new Rickshaw.Graph({
                element: document.getElementById('chart'),
                series: [
                    {
                    color: 'steelblue',
                    data: [ { x: 0, y: 23}, { x: 1, y: 15 }, { x: 2, y: 79 } ]
                    }, {
                    color: 'lightblue',
                    data: [ { x: 0, y: 30}, { x: 1, y: 20 }, { x: 2, y: 64 } ]
                    }
                ]
                });
            graph.render();
            }
        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
    margin: 40px 0 0;
}
ul {
    list-style-type: none;
    padding: 0;
}
li {
    display: inline-block;
    margin: 0 10px;
}
a {
    color: #42b983;
}
</style>
