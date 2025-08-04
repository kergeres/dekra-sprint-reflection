<template>
  <header class="bg-[#ffffff] text-white w-full fixed top-0 left-0 flex justify-between items-center px-6 py-2 shadow-md z-50">
    <h2 class="text-xl text-[black] font-bold tracking-wide">
      Sprint Reflection 2025 {{ arrow }}
    </h2>
    <nav class= "flex gap-6 text-white">
      <button @click="sprintDataChange('25.2')" :class="displayedSprint === '25.2' && 'underline'" class=" !text-[green] hover:underline transition-colors">25.2</button>
      <button @click="sprintDataChange('25.1')" :class="displayedSprint === '25.1' && 'underline'" class="hover:underline transition-colors">25.1</button>
      <button @click="sprintDataChange('24.4')" :class="displayedSprint === '24.4' && 'underline'" class="hover:underline transition-colors">24.4</button>

      <button @click="sprintDataChange('summary')" :class="displayedSprint === 'summary' && 'underline'" class="hover:underline summary transition-colors">Summary</button>
    </nav>
  </header>
</template>

<script setup>
import { ref } from "vue";

const displayedSprint = ref('summary');

const emit = defineEmits(["sprint-change"]);

const sprintDataChange = (sprint) => {
  console.log('sprint', sprint)
  displayedSprint.value = sprint
  emit("sprint-change", sprint);
};
</script>