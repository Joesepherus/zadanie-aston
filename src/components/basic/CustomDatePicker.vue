<template lang="html">
  <div class="example">
    <span><span class="required">*</span>{{label}}</span>
    <datepicker v-model="date" v-bind:placeholder="placeholder"></datepicker>
  </div>
</template>
    
<script>
import Datepicker from "vuejs-datepicker";
export default {
  data() {
    return {
      date: this.defaultDate
    };
  },
  props: {
    placeholder: String,
    label: String,
    type: String,
    defaultDate: Date
  },
  watch: {
    date: function(value) {
      this.$emit("change", value, this.type);
    }
  },
  components: {
    Datepicker
  }
};
</script> 

<style>
body {
  font-family: "Helvetica Neue Light", Helvetica, sans-serif;
  padding: 1em 2em 2em;
}
input,
select {
  padding: 0.75em 0.5em;
  font-size: 100%;
  /* border: 1px solid #ccc; */
  width: 100%;
}

select {
  height: 2.5em;
}

.example input {
  border: 1px solid rgba(34, 36, 38, 0.15);
  border-radius: 3px;
  outline: none;
}
</style>