<template lang="html">
  <div>
    <sui-checkbox v-bind:label="label" toggle v-model="value"/>
  </div>
</template>

<script>
export default {
  name: "CheckboxToggleExample",
  props: {
    label: String,
    type: String,
    defaultValue: Boolean
  },
  data() {
    return { value: this.defaultValue };
  },
  watch: {
    value: function(value) {
      this.$emit("change", value, this.type);
    }
  }
};
</script>

<style lang="css">
</style>
