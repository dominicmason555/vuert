<template>
    <div class="hello">
        <h1>{{ msg }}</h1>
        <div id="chart_container">
        <div id="chart" ref="chart" class="rickshaw-graph"></div>
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
        data: function () {
            return {
                lastNum: 0,
                lastX: 100,
                lightData: Array.from({length: 100}, (x, i) => ({x: i, y: 0})),
                zeroAxis: Array.from({length: 100}, (x, i) => ({x: i, y: 0})),
                graph: []
            }
        },
        mounted() {
            this.initChart();
            setInterval(() => {
                this.lastX += 1;
                this.lastNum += Math.random() - 0.5;
                this.lightData.shift();
                this.lightData.push({x: this.lastX, y: this.lastNum});
                this.zeroAxis.shift();
                this.zeroAxis.push({x: this.lastX, y: 0});
                this.graph[0].update();
            }, 100);
        },
        methods: {
            initChart() {
                this.graph[0] = new Rickshaw.Graph({
                element: this.$refs.chart,
                height: 300,
                width: 800,
                min: "auto",
                padding: {
                    top: 0.1,
                    bottom: 0.1,
                    left: 0.1,
                    right: 0.1
                },
                renderer: "line",
                series: [
                    {
                        color: 'black',
                        data: this.lightData
                    },
                    {
                        color: 'gray',
                        data: this.zeroAxis
                    }
                ]
                });
                this.graph[0].render();
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
