
<template>
  <h1>
    {{ title }}
  </h1>
  <Toolbar
    :month="selectedMonth"
    :year="yearOptionIndex"
    :month-drop-down-options="monthDropDownOptions"
    :year-drop-down-options="yearDropDownOptions"
    @monthUpdateOption="handleMonthUpdate"
    @yearUpdateOption="handleYearUpdate"
   />
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
      selectedYear: 2022,
      yearOptionIndex: 22,
      yearDropDownOptions: []
    }
  },
  computed: {
    title() {
      return `Daily Prices for Stock Symbol ${this.symbol} on ${this.monthName} ${this.selectedYear}`;
    },
    filteredData() {
      return this.stockData[this.selectedYear][this.selectedMonth];
    },
    monthName() {
      return Months[this.selectedMonth];
    },
    monthDropDownOptions() {
      const monthOptions = [];
      // only create options if month data exists
      Months.forEach((monthName, index) => {
        if (this.stockData[this.selectedYear][index]) {
          monthOptions.push({
            name: monthName,
            number: index
          });
        }
      });
      return monthOptions;
    }
  },
  mounted() {},
  created() {
    this.stockMetadata = stockData[KEYS.METADATA];
    this.symbol = this.stockMetadata[KEYS.SYMB];
    // parse data from JSON file
    this.parseJSONData(stockData[KEYS.DAILY]);

    this.yearDropDownOptions = this.createArrayOfYears(1999, 2022);
    this.yearOptionIndex = this.findYearOptionIndex(this.selectedYear);
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
    },
    handleYearUpdate(option) {
      this.selectedYear = Number(option.name)
      this.yearOptionIndex = this.findYearOptionIndex(this.selectedYear);
    },
    createArrayOfYears(startYear, endYear) {
      // return Array(endYear - startYear + 1)
      // .fill(startYear)
      // .map((year, index) => {
      //   const obj = {
      //     "name": `${year + index}`,
      //     "number": index
      //   }
      //   return obj
      // });
       return Array(endYear - startYear + 1)
      .fill(endYear)
      .map((year, index) => {
        const obj = {
          "name": `${year - index}`,
          "number": index
        }
        return obj
      });
    },
    findYearOptionIndex(year) {
      return this.yearDropDownOptions.findIndex((option, index) => {
          return option.name === year.toString()
        });
    }
  }
}
</script>

<style>
</style>
