<template>
  <div class="screen">
    <h2>✨ Congratulations ✨</h2>
    <h3>{{ Math.round(timer / 1000) }} seconds</h3>
    <button @click="onStartAgain">Start Again</button>
  </div>
</template>

<script>
export default {
  props: {
    timer: {
      type: Number,
    },
  },
  methods: {
    onStartAgain() {
      this.$emit("onStartAgain");
    },
  },
};
</script>

<style></style>
