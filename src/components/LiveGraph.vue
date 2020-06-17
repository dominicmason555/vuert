<template>
    <div class="LiveGraph">
        <h2>{{ msg }}</h2>
        <div id="chart_container">
            <div id="chart" ref="chart" class="rickshaw-graph"></div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'LiveGraph',
        props: {
            msg: String,
            dataKey: String
        },
        data: function () {
            return {
                lightData: Array.from({length: 100}, 
                                        () => ({x: Date.now(), y: 0})),
                zeroAxis: Array.from({length: 100}, 
                                        () => ({x: Date.now(), y: 0})),
                graph: []
            }
        },
        mounted() {
            this.initChart();
            this.$root.$on("graphUpdate", (dataset) => {
                if (this.dataKey in dataset) {
                    this.addDatapoint(dataset[this.dataKey]);
                }
            });
        },
        methods: {
            addDatapoint(point) {
                this.lightData.shift();
                this.lightData.push({x: Date.now(), y: point});
                this.zeroAxis.shift();
                this.zeroAxis.push({x: Date.now(), y: 0});
                this.graph[0].update();
            },
            initChart() {
                this.graph[0] = new this.$rickshaw.Graph({
                    element: this.$refs.chart,
                    min: "auto",
                    padding: {
                        top: 0.1,
                        bottom: 0.1,
                        left: 0,
                        right: 0
                    },
                    renderer: "line",
                    interpolation: "basis",
                    series: [
                        {color: '#56b6c2', data: this.lightData},
                        {color: '#abb2bf', data: this.zeroAxis}
                    ]
                });
                this.graph[0].render();
            }
        }
    }
</script>

<style scoped>

.rickshaw-graph {
    border: 2px solid #5c6370;
    border-radius: 10px;
    box-shadow: 0 0 10px 10px rgba(0, 0, 0, 0.1);
}

</style>
