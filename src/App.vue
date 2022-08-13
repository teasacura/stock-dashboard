
<template>
  <h1>
    {{ title }}
  </h1>
  <ToolbarVue :month="selectedMonth" @updateOption="handleUpdate" />
  <MainContentVue :month="selectedMonth" :stock-data="filteredData" />
  <div>

  </div>
</template>

<script>
import dayjs from "https://cdn.skypack.dev/dayjs";
import stockData from "./data/IBM-daily-data.json";

import ToolbarVue from "./components/Toolbar.vue";
import MainContentVue from "./components/MainContent/MainContent.vue";

import { KEYS, Months } from "./constants.js";

export default {
  name: "ProjectApp",
  components: {
    ToolbarVue,
    MainContentVue
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
    handleUpdate(option) {
      this.selectedMonth = option.number;
    }
  }
}
</script>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
