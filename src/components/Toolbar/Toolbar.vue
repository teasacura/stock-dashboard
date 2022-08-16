<template>
  <div>
    <Dropdown 
      ref="month-dropdown"
      :selected-index="month"
      :drop-down-options="monthDropDownOptions"
      @updateOption="emitMonthUpdate"
      />
  </div>
  <!-- <Dropdown ref="year-dropdown"/> -->
</template>

<script>

import Dropdown from "./components/Dropdown.vue";
import { Months } from "../../constants.js";

export default {
  name: "Toolbar",
  components: {
    Dropdown
  },
  props: {
    month: {
      type: Number,
      required: false
    }
  },
  data() {
    return {
        monthDropDownOptions: [],
        yearDropDownOptions: []
    }
  },
  computed: {
  },
  mounted() {
  },
  created() {
    this.monthDropDownOptions = Months.map((monthName, index) => {
      return {
        name: monthName,
        number: index
      }
    });
  },
  methods: {
    emitMonthUpdate(option) {
      this.$emit('monthUpdateOption', option);
    }
  }
}
</script>

<style scoped></style>