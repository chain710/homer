<template>
  <div>
    <div
      class="card"
      :style="`background-color:${item.background};`"
      :class="item.class"
    >
      <p class="title">{{ item.name }}</p>
      <video autoplay muted id="player"></video>
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
  data: () => ({
    speedtest: null,
  }),
  computed: {},
  created() {},
  mounted: function () {
    this.jmuxer = new JMuxer({
      node: "player",
      mode: "video",
      flushingTime: 0,
      fps: 30,
      debug: false,
    });
  },
  methods: {},
};
</script>
