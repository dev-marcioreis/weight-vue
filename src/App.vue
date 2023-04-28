<script setup>

  import { ref, shallowRef, computed, watch, nextTick } from 'vue'
  import { Chart } from 'chart.js';
  
  const weights = ref([])
  const weightChartElement = ref(null)
  const weightChart = shallowRef(null)
  const weightInput = ref(60.0)

  const currentWeight = computed(() => {
    return weights.value.sort((a, b) => b.date - a.date)[0] || { weight: 0 }
  })

  const addWeight = () => {
    weights.value.push({
      weight: weightInput.value,
      date: new Date().getTime()
    })
  }

</script>

<template>
  <main>
    <h1>Controle de peso</h1>
    <div class="current">
      <span>{{ currentWeight.weight }}</span>
      <small>Peso atual (kg)</small>
    </div>
    <form @submit.prevent="addWeight">
      <input type="number" step="0.1" v-model="weightInput" />
      <input type="submit" value="Adicionar" />
    </form>
    <div v-if="weights && weights.length > 0">
      <h2>Últimos 7 dias</h2>
      <div class="canvas-box">
        <canvas ref="weightChartElement"></canvas>
      </div>
      <div class="weight-history">
        <h2>Hitórico</h2>
        <ul>
          <li v-for="weight in weights">
            <span>{{ weight.weight }}kg</span>
            <small>{{ new Date(weight.date).toLocaleDateString() }}</small>
          </li>
        </ul>
      </div>
    </div>
  </main>
</template>

<style>

</style>
