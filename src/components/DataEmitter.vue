<template>
    <div class="DataEmitter">
        <h2>{{msg}} at {{frequency}} Hz</h2>
        <h2>{{this.connection == null ? "Not Connected" : "Connected"}}</h2>
        <button v-on:click="connect()">Connect</button>
        <button v-on:click="sendMessage('hello')">Send Message</button>
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
                interval: Number,
                connection: null
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
            },
            sendMessage(message) {
                console.log("Hello");
                console.log(this.connection);
                this.connection.send(message);
            },
            connect() {
                console.log("Starting connection to WebSocket Server");
                this.connection = new WebSocket("wss://localhost:5001/ws");

                this.connection.onmessage = function(event) {
                    console.log(event);
                }

                this.connection.onopen = function(event) {
                    console.log(event);
                    console.log("Successfully connected to websocket server...");
                }
            }
        }
    }
</script>

<style scoped>

h2 {
    color: #d4d4d4;
}

</style>
