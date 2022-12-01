<template>
  <div class="mesage is-dark">
    <div class="card" :class="item.class">
      <div v-if="item.name" class="message-header is-dark">
        <p>{{ item.name }}</p>
      </div>
      <video autoplay muted ref="player"></video>
    </div>
  </div>
</template>
<script>
import JMuxer from "jmuxer";

export default {
  name: "VideoStream",
  props: {
    item: Object,
  },
  mounted: function () {
    console.log(`video streamer mount ${this.jmuxer}`);
    this.jmuxer = new JMuxer({
      node: this.$refs.player,
      mode: "video",
      flushingTime: 0,
      fps: 30,
      debug: false,
    });
    var socketURL = document.location.href.replace("http", "ws") + "ws/";
    console.log(`connect ${socketURL}`);
    this.ws = new WebSocket(socketURL);
    this.ws.binaryType = "arraybuffer";
    let vm = this;
    this.ws.addEventListener("message", function (event) {
      const data = vm.parse(event.data);
      vm.jmuxer.feed(data);
    });
  },
  beforeUnmount: function () {
    console.log(`video streamer unmount`);
    if (this.jmuxer) {
      this.jmuxer.destroy();
      this.jmuxer = null;
    }
    if (this.ws) {
      this.ws.close();
      this.ws = null;
    }
  },
  methods: {
    parse: function (data) {
      let input = new Uint8Array(data),
        video = input;

      return {
        video: video,
      };
    },
  },
};
</script>
<style scoped>
.container video {
  display: block;
}
</style>

<style lang="scss" scoped>
@import "../../node_modules/bulma/bulma";
.video-title {
  color: var(--text-title);
  font-size: 1.1em;
  line-height: 1.2em;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  font-weight: bold;
  padding: 0.75em 1em;
}
</style>
