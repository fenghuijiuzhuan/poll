<template>
  <div id="app">
    {{ count }}
  </div>
</template>

<script>
import Poll from "./poll.js";
export default {
  name: "App",
  data() {
    return {
      count: 0,
      timer: null,
      list: [],
    };
  },
  mounted() {
    this.getData();
    this.createTimer();
  },
  activated() {
    this.clearTimer();
    this.$nextTick(() => {
      this.timer.goon.call(this.timer);
    });
  },
  deactivated() {
    this.clearTimer();
  },
  beforeDestroy() {
    this.clearTimer();
  },
  methods: {
    getData() {
      return new Promise((resolve) => {
        setTimeout(() => {
          this.count++;
          resolve()
        }, 2000);
      });
    },
    createTimer() {
      this.timer = Poll.start({
        interval: 3 * 1000,
        action: () => this.getData(),
      });
      this.timer.run.call(this.timer);
    },
    clearTimer() {
      Poll.stop(this.timer);
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
</style>
