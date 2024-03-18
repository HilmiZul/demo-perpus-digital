<template>
  <div>
    <canvas id="myChart"></canvas>
  </div>
</template>

<script setup lang="ts">
import Chart from 'chart.js/auto'

const labels = [
  'Januari',
  'Pebruari',
  'Maret',
  'April',
  'Mei',
  'Juni',
  'Juli',
];

const data = {
  labels: labels,
  datasets: [{
    label: 'Pengunjung',
    backgroundColor: 'rgb(0, 153, 255, 100)',
    borderColor: 'rgb(255, 99, 132)',
    data: [20, 10, 5, 2, 20, 50, 77],
  }]
};

const config: ChartConfiguration = {
  type: 'bar',
  data: data,
  options: {}
};

onMounted(() => {
  const canvas = <ChartItem>document.getElementById('myChart')
  new Chart(
    canvas,
    config
  );
})
</script>

