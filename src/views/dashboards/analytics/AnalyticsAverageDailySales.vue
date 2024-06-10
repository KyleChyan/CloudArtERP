<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'
import { useTheme } from 'vuetify'
import api from '@/utils/api'

const vuetifyTheme = useTheme()
const currentTheme = vuetifyTheme.current.value.colors

const series = [{
  data: [
    400,
    200,
    650,
    500,
  ],
}]

const chartOptions = {
  chart: {
    type: 'area',
    toolbar: { show: false },
    sparkline: { enabled: true },
  },
  markers: {
    colors: 'transparent',
    strokeColors: 'transparent',
  },
  grid: { show: false },
  colors: [currentTheme.success],
  fill: {
    type: 'gradient',
    gradient: {
      shadeIntensity: 0.8,
      opacityFrom: 0.6,
      opacityTo: 0.1,
    },
  },
  dataLabels: { enabled: false },
  stroke: {
    width: 2,
    curve: 'smooth',
  },
  xaxis: {
    show: true,
    lines: { show: false },
    labels: { show: false },
    stroke: { width: 0 },
    axisBorder: { show: false },
  },
  yaxis: {
    stroke: { width: 0 },
    show: false,
  },
  responsive: [
    {
      breakpoint: 1387,
      options: { chart: { height: 80 } },
    },
    {
      breakpoint: 1200,
      options: { chart: { height: 120 } },
    },
  ],
}

const data = ref('$28,450')

onMounted(async () => {
  try {
    const response = await axios.get('http://localhost:8081/api/v1/public/getProportion')

    data.value = `${response.data}%`
  } catch (error) {
    console.error('Error fetching sales data:', error)
  }
})
</script>

<template>
  <VCard>
    <VCardText>
      <h5 class="text-h5 mb-3">
        Average Daily Sales
      </h5>
      <p class="mb-0">
        成交订单百分比
      </p>
      <h4 class="text-h4">
        {{ data }}
      </h4>
    </VCardText>

    <VueApexCharts
      :options="chartOptions"
      :series="series"
      :height="80"
    />
  </VCard>
</template>
