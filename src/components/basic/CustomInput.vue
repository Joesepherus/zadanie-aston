<template lang="html">
  <div class="input">
    <p><span class="required">*</span>{{placeholder}}</p>
    <sui-input 
      v-bind:placeholder="placeholder" 
      v-model="data"
    />
  </div>
</template>

<script>
export default {
  name: "InputExample",
  props: {
    placeholder: String,
    type: String,
    defaultValue: String
  },
  data() {
    return {
      data: this.defaultValue
    };
  },
  watch: {
    data: function(value) {
      this.$emit("change", value, this.type);
    }
  }
};
</script>

<style>
.input {
  width: 100%;
}
</style>