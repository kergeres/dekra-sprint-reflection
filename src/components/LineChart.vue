<template>
  <h2 class="text-[black] text-xl mt-8 mb-4">Sprint {{ sprintTitle }}</h2>
  <Line :data="chartData" :options="chartOptions" class="w-[800px] h-[400px]" />
  <div
  class="height-[30px]"
  >
  </div>
  <p v-if="isSummary" class="text-[black]">{{ average }}</p>
</template>

<script setup>
import { computed, ref } from 'vue'
import { Line } from 'vue-chartjs'
import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  LineElement,
  PointElement,
  CategoryScale,
  LinearScale
} from 'chart.js'

ChartJS.register(Title, Tooltip, Legend, LineElement, PointElement, CategoryScale, LinearScale)

const props = defineProps({
  dataObj: {
    type: Object,
    required: true
  }
})

const sprintTitle = computed(() => {
  const firstLabel = props.dataObj.labels?.[0] || ''
  const prefix = firstLabel.split('-')[0]
  return props.dataObj.labels.length > 7 ? 'Summary' : `- ${prefix}`
})

const isSummary = computed(() => sprintTitle.value === 'Summary')

const average = computed(() => {
  const arr = props.dataObj.completed || []
  if (!arr.length) return 0
  const sum = arr.reduce((acc, val) => acc + val, 0)
  return (sum / arr.length).toFixed(2)
})

const chartData = computed(() => ({
  labels: props.dataObj.labels,
  datasets: [
    {
      label: 'planned',
      data: props.dataObj.planned,
      borderColor: '#8acb5b',
      backgroundColor: '#8acb5b50',
      pointBackgroundColor: '#8acb5b',
      pointBorderColor: '#8acb5b',
      fill: true,
      tension: 0
    }, {
      label: 'completed',
      data: props.dataObj.completed,
      borderColor: '#008143',
      backgroundColor: '#64B32C50',
      pointBackgroundColor: '#006B52',
      pointBorderColor: '#006B52',
      fill: true,
      tension: 0
    },
  ]
}))

const chartOptions = ref({
  responsive: true,
  plugins: {
    legend: {
      display: true,
      position: 'bottom',
      labels: {
        color: '#002E55'
      }
    },
    tooltip: {
      backgroundColor: '#002E55',
      titleColor: '#FFCC00',
      bodyColor: '#FFFFFF'
    }
  },
  scales: {
    y: {
      ticks: { color: '#40444a' },
      grid: { color: 'gray' }
    },
    x: {
      ticks: { color: '#40444a' },
      grid: { display: false }
    }
  }
})
</script>
