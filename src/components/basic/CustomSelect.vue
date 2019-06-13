<template lang="html">
  <div>
    <p><span class="required">*</span>{{label}}</p>
    <sui-dropdown
      v-bind:placeholder="placeholder"
      selection
      v-bind:options="options"
      v-model="data"
    />
  </div>
</template>

<script>
export default {
  name: "SelectionExample",
  props: {
    label: String,
    placeholder: String,
    type: String,
    options: Array
  },
  watch: {
    data: function(value) {
      this.$emit("change", value, this.type);
    }
  },
  data() {
    return {
      data: null
      //   options: [
      //     {
      //       text: "Male",
      //       value: 1
      //     },
      //     {
      //       text: "Female",
      //       value: 2
      //     }
      //   ]
    };
  }
};
</script>