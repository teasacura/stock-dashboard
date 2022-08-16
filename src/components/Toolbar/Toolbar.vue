<template>
  <div class="toolbar-container">
    <Dropdown
      ref="month-dropdown"
      :selected-index="month"
      :drop-down-options="monthDropDownOptions"
      @updateOption="emitMonthUpdate"
      />
    <Dropdown
      ref="year-dropdown"
      :selected-index="year"
      :drop-down-options="yearDropDownOptions"
      @updateOption="emitYearUpdate"
      />
  </div>
</template>

<script>

import Dropdown from "./components/Dropdown.vue";

export default {
  name: "Toolbar",
  components: {
    Dropdown
  },
  props: {
    month: {
      type: Number,
      required: false
    },
    year: {
      type: Number,
      required: false
    },
    monthDropDownOptions: {
      type: Array,
      required: false
    },
    yearDropDownOptions: {
      type: Array,
      required: false
    }
  },
  data() {
    return {
    }
  },
  computed: {
  },
  mounted() {
  },
  created() {
  },
  methods: {
    emitMonthUpdate(option) {
      this.$emit("monthUpdateOption", option);
    },
    emitYearUpdate(option) {
      this.$emit("yearUpdateOption", option);
    }
  }
}
</script>

<style scoped>
  .toolbar-container {
    display: flex;
  }
</style>