<template>
    <div class="DataEmitter">
        <h2>{{msg}}</h2>
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
        },
        data: function () {
            return {
                lastNum: 0,
                counter: 0,
                connection: null
            }
        },
        methods: {
            sendMessage(message) {
                console.log("Hello");
                console.log(this.connection);
                this.connection.send(message);
            },
            connect() {
                console.log("Starting connection to WebSocket Server");
                this.connection = new WebSocket("wss://localhost:5001/ws");

                var self = this;
                this.connection.onmessage = function(event) {
                    var dataset = JSON.parse(event.data)
                    self.$root.$emit("graphUpdate", dataset);
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
