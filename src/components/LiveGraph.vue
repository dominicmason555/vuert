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
                        left: 0.1,
                        right: 0.1
                    },
                    renderer: "line",
                    series: [
                        {color: 'black', data: this.lightData},
                        {color: 'gray', data: this.zeroAxis}
                    ]
                });
                this.graph[0].render();
            }
        }
    }
</script>

<style scoped>

</style>
