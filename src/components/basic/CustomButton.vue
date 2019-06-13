<template lang="html">
  <div>
    <sui-button primary v-on:click="click()">{{text}}</sui-button>
  </div>
</template>

<script>
export default {
  name: "ButtonEmphasisExample",
  props: {
    text: String
  },
  methods: {
    click() {
      this.$emit("click");
    }
  }
};
</script>
