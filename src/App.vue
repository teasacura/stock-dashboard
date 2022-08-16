
<template>
  <h1>
    {{ title }}
  </h1>
  <Toolbar :month="selectedMonth" @monthUpdateOption="handleMonthUpdate" />
  <MainContent :month="selectedMonth" :stock-data="filteredData" />
  <div>

  </div>
</template>

<script>
import dayjs from "https://cdn.skypack.dev/dayjs";
import stockData from "./data/IBM-daily-data.json";

import Toolbar from "./components/Toolbar/Toolbar.vue";
import MainContent from "./components/MainContent/MainContent.vue";

import { KEYS, Months } from "./constants.js";

export default {
  name: "ProjectApp",
  components: {
    Toolbar,
    MainContent
  },
  data() {
    return {
      symbol: null,
      stockMetadata: null,
      stockData: {},
      selectedMonth: 6,
      selectedYear: 2021
    }
  },
  computed: {
    title() {
      return `Daily Prices for Stock Symbol ${this.symbol} on ${Months[this.selectedMonth]} ${this.selectedYear}`;
    },
    filteredData() {
      return this.stockData[this.selectedYear][this.selectedMonth];
    }
  },
  mounted() {},
  created() {
    this.stockMetadata = stockData[KEYS.METADATA];
    this.symbol = this.stockMetadata[KEYS.SYMB];
    // parse data from JSON file
    this.parseJSONData(stockData[KEYS.DAILY]);
  },
  methods: {
    parseJSONData(jsonData) {
      // create hash stored in Decade, Year, Month
      for (const dateString in jsonData) {
          const entryDate = dayjs(dateString, 'YYYY-MM-DD');
          const entryDay = entryDate.date();
          const entryYear = entryDate.year();
          const entryMonth = entryDate.month();
          if (!this.stockData[entryYear]) {
            this.stockData[entryYear] = {};
          }
          if (!this.stockData[entryYear][entryMonth]) {
            this.stockData[entryYear][entryMonth] = {};
          }
          this.stockData[entryYear][entryMonth][entryDay] = jsonData[dateString];
      }
    },
    handleMonthUpdate(option) {
      this.selectedMonth = option.number;
    }
  }
}
</script>

<style>
</style>
