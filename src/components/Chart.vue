<template>
  <div class="container">
    <Line v-if="loaded" :data="chartData" :options="chartOptions" />
  </div>
</template>

<script>
import { Line } from "vue-chartjs";
import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  LineElement,
  CategoryScale,
  LinearScale,
  PointElement,
} from "chart.js";

ChartJS.register(
  Title,
  Tooltip,
  Legend,
  LineElement,
  CategoryScale,
  LinearScale,
  PointElement
);

export default {
  name: "BarChart",
  components: { Line },
  data: () => ({
    loaded: false,
    chartData: null,
    chartOptions: {
      animations: {
        tension: {
          duration: 1000,
          easing: "linear",
        },
      },
    },
  }),
  async mounted() {
    this.loaded = false;

    const api1 = "https://pokeapi.co/api/v2/pokemon/charizard";
    const api2 = "https://pokeapi.co/api/v2/pokemon/blastoise";
    const api3 = "https://pokeapi.co/api/v2/pokemon/venusaur";

    try {
      const [res1, res2, res3] = await Promise.all([
        fetch(api1),
        fetch(api2),
        fetch(api3),
      ]);

      const [data1, data2, data3] = await Promise.all([
        res1.json(),
        res2.json(),
        res3.json(),
      ]);

      console.log(data1, data2, data3);

      this.chartData = {
        labels: data1.game_indices.map((index) => index.version.name),
        datasets: [
          {
            label: data1.name,
            backgroundColor: "#f03e3e",
            data: data1.game_indices.map((index) => index.game_index),
          },
          {
            label: data2.name,
            backgroundColor: "#339af0",
            data: data2.game_indices.map((index) => index.game_index),
          },
          {
            label: data3.name,
            backgroundColor: "#51cf66",
            data: data3.game_indices.map((index) => index.game_index),
          },
        ],
      };

      this.loaded = true;
    } catch (e) {
      console.error(e);
    }
  },
};
</script>

<style scoped>
.container {
  max-width: 1600px;
  margin: 0 auto;
}
</style>
