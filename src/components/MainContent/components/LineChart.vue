<template>
  <div class="LineChart">
    <Line 
      :chart-options="chartOptions"
      :chart-data="chartData"
      :chart-id="chartId"
      :dataset-id-key="datasetIdKey"
      :plugins="plugins"
      :css-classes="cssClasses"
      :styles="styles"
      :width="width"
      :height="height"
    />
  </div>
</template>

<script>
import { KEYS, Months } from "../../../constants.js";
import { Line } from 'vue-chartjs';

import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  LineElement,
  LinearScale,
  CategoryScale,
  PointElement
} from 'chart.js'

ChartJS.register(
  Title,
  Tooltip,
  Legend,
  LineElement,
  LinearScale,
  CategoryScale,
  PointElement
)

export default {
  name: "LineChart",
  components: {
    Line
  },
  props: {
    stockData: {
      type: Object,
      required: false
    },
    month: {
      type: Number,
      required: false
    },
    chartId: {
      type: String,
      default: 'line-chart'
    },
    datasetIdKey: {
      type: String,
      default: 'label'
    },
    width: {
      type: Number,
      default: 400
    },
    height: {
      type: Number,
      default: 400
    },
    cssClasses: {
      default: '',
      type: String
    },
    styles: {
      type: Object,
      default: () => {}
    },
    plugins: {
      type: Array,
      default: () => []
    }
  },
  data() {
    return {
      title: "LineChart HERE!",
      stockDataOptions: [
        {
          label: "Open",
          backgroundColor: "#0040ff",
          dataName: "stockOpen"
        },
         {
          label: "High",
          backgroundColor: "#00ff00",
          dataName: "stockHigh"
        },
         {
          label: "Low",
          backgroundColor: "#ff0000",
          dataName: "stockLow"
        },
         {
          label: "Close",
          backgroundColor: "#ffa500",
          dataName: "stockClose"
        }
      ],
      chartOptions: {
        responsive: true,
        maintainAspectRatio: false
      }
    }
  },
  computed: {
    dataLabels() {
      const monthName = Months[this.month];
      const monthDate = Object.keys(this.stockData);
      return monthDate.map((date) => {
        return monthName + " " + date;
      });
    },
    chartData() {
      return {
        labels: this.dataLabels,
        datasets: this.stockDataSets
      }
    },
    structuredData() {
      const stockOpen = [];
      const stockHigh = [];
      const stockLow = [];
      const stockClose = [];
      for (let day in this.stockData) {
        stockOpen.push(this.stockData[day][KEYS.OPEN]);
        stockHigh.push(this.stockData[day][KEYS.HIGH]);
        stockLow.push(this.stockData[day][KEYS.LOW]);
        stockClose.push(this.stockData[day][KEYS.CLOSE]);
      }

      return { 
        stockOpen,
        stockHigh,
        stockLow,
        stockClose
      }
    },
     stockDataSets() {
      const dataSets = [];

      this.stockDataOptions.forEach((option) => {
        const dataset = {
          label: `Stock ${option.label}`,
          backgroundColor: option.backgroundColor,
          data: this.structuredData[option.dataName]
        };

        dataSets.push(dataset);
      });
      return dataSets;
    }
  },
  mounted() {
  },
  methods: {
  }
}
</script>

<style scoped>
  .LineChart {
    background-color: white;
  }
</style>