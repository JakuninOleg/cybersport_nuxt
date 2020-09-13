<template>
  <GChart
    :type="chartType"
    :data="chartData"
    :options="chartOptions"
    :settings="{ packages: ['line'] }"
    :create-chart="(el, google) => new google.charts.Line(el)"
  />
</template>

<script>
import { GChart } from 'vue-google-charts'
export default {
  components: {
    GChart
  },
  props: {
    chartData: {
      type: Array,
      required: true,
      default: () => {
        return {}
      }
    }
  },
  data () {
    return {
      chartType: 'Line',
      chartOptions: {
        title: 'Mouse dx/dy',
        curveType: 'function',
        legend: { position: 'bottom' },
        width: 900,
        height: 500,
        series: {
          // Gives each series an axis name that matches the Y-axis below.
          0: { axis: 'Mouse_dx' },
          1: { axis: 'Mouse_dy' }
        },
        axes: {
          // Adds labels to each axis; they don't have to match the axis names.
          y: {
            Temps: { label: 'Mouse_dx)' },
            Daylight: { label: 'Mouse_dy' }
          }
        }
      }
    }
  }
}
</script>
