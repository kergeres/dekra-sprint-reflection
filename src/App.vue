<template>
  <TheHeader @sprint-change="handleSprintChange" />
  <LineChart :dataObj="displayedDataObj" />
</template>

<script setup>
import { ref } from 'vue'
import LineChart from './components/LineChart.vue'
import TheHeader from './components/TheHeader.vue'

const data_24_4 = {
  labels: ['24.4-1', '24.4-2', '24.4-3', '24.4-5'],
  planned: [18, 23, 38, 22],
  completed: [16, 26, 38, 22] 
}

const data_25_1 = {
  labels: ['25.1-1', '25.1-2', '25.1-3', '25.1-4', '25.1-5', '25.1-6'],
  planned: [27, 23, 27, 21, 24, 22],
  completed: [19, 23, 3, 44, 10, 27]
}

const data_25_2 = {
  labels: ['25.2-1', '25.2-2', '25.2-3', '25.2-4',],
  planned: [18, 18, 20, 20, 26],
  completed: [0, 18, 8, 0,]
}

const dataMap = {
  data_24_4,
  data_25_1,
  data_25_2,
}
const mergeAllData = () => {
  const merged = {
    labels: [],
    planned: [],
    completed: []
  }

  Object.values(dataMap).forEach(obj => {
    merged.labels.push(...obj.labels)
    merged.planned.push(...obj.planned)
    merged.completed.push(...obj.completed)
  })

  return merged
}
const displayedDataObj = ref(mergeAllData())

const handleSprintChange = (val) => {
  if (val === 'summary') {
    displayedDataObj.value = mergeAllData()
  } else {
    const key = `data_${val.replace('.', '_')}`
    if (dataMap[key]) {
      displayedDataObj.value = dataMap[key]
    } else {
      console.warn('Nincs ilyen adat:', key)
    }
  }
}
</script>
