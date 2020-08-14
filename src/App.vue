<template>
  <div id="app">
    <button @click="sendMessage('add')" class="add">+</button>

    <h1>{{count}}</h1>
    <button @click="sendMessage('subtract')" class="subtract">-</button>
  </div>
</template>

<script>
export default {
  name: "App",
  components: {},
  data() {
    return {
      count: 0,
      connection: null,
    };
  },
  created: function () {
    console.log("Starting connection to WebSocket Server");
    this.connection = new WebSocket("ws://35.232.244.207:8096");
    let aa = this;
    this.connection.onmessage = function (event) {
      console.log(event.data);
      if (event.data == "add") {
        aa.add();
      } else if (event.data == "subtract") {
        aa.subtract();
      }
    };

    this.connection.onopen = function (event) {
      console.log(event);
      console.log("Successfully connected to the echo websocket server...");
    };
  },
  methods: {
    add() {
      this.count += 1;
    },
    subtract() {
      this.count -= 1;
      if (this.count < 0) {
        this.count = 0;
      }
    },
    sendMessage: function (message) {
      console.log(this.connection);
      this.connection.send(message);
    },
  },
};
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
.subtract {
  color: white;
  background-color: red;
  font-size: 10em;
  padding: 40px;
}
.add {
  color: white;
  background-color: green;
  font-size: 10em;
  padding: 20px;
}
</style>
