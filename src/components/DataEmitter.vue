<template>
    <div class="DataEmitter">
        <h2>{{msg}} at {{frequency}} Hz</h2>
    </div>
</template>

<script>
    export default {
        name: 'DataEmitter',
        props: {
            msg: String,
            frequency: Number
        },
        data: function () {
            return {
                lastNum: 0,
                counter: 0,
                interval: Number
            }
        },
        mounted() {
            this.interval = setInterval(this.addDataset, 1000 / this.frequency);
        },
        beforeDestroy() {
            clearInterval(this.interval);
        },
        methods: {
            addDataset() {
                this.lastNum += Math.random() - 0.5;
                var dataset = {"point1": this.lastNum};
                if (this.counter >= 5) {
                    dataset["point2"] = this.lastNum;
                    this.counter = 0;
                }
                this.counter += 1;
                this.$root.$emit("graphUpdate", (dataset));
            }
        }
    }
</script>

<style scoped>

</style>
