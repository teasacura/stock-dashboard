<template>
  <div>  
    <LineChart :month="month" :stock-data="stockData" />
  </div>

</template>

<script>
import LineChart from './components/LineChart.vue';

export default {
  name: "ProjectApp",
  components: {
    LineChart
  },
  props: {
    stockData: {
      type: Object,
      required: false
    },
    month: {
      type: Number,
      required: false
    }
  },
  data() {
    return {
      title: "MAIN CONTENT HERE!"
    }
  },
  computed: {}
}
</script>

<style scoped>
</style>