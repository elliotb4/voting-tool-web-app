<template>
  <div class="table">
    <h1>Preflib Election Simulator</h1>
    <table>
      <tr class="top">
        <th>No. of Candidates</th>
        <td>{{ results.candidates }}</td>
      </tr>
      <tr>
        <th>Borda Count</th>
        <td>{{ results.borda }}</td>
      </tr>
      <tr>
        <th>Instant Runoff</th>
        <td>{{ results.irv }}</td>
      </tr>
      <tr>
        <th>Single Transferrable Vote</th>
        <td>{{ results.stv }}</td>
      </tr>
      <tr>
        <th>Plurality</th>
        <td>{{ results.plurality }}</td>
      </tr>
      <tr>
        <th>Condorcet Winner</th>
        <td>{{ results.condorcet }}</td>
      </tr>
    </table>
    <div class="select">
      <label for="dataset">Dataset: </label>
      <input
        v-model.number="dataset"
        @change="fetchData(dataset)"
        type="number"
        id="dataset"
        name="dataset"
        min="1"
        max="87"
      />
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      results: [],
      dataset: 1,
    };
  },
  mounted() {
    this.fetchData(this.dataset);
  },
  methods: {
    fetchData(dataset) {
      axios
        .post("http://localhost:8080/preflib", {
          set: dataset,
        })
        .then((res) => {
          console.log(res.data);
          this.results = res.data;
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
};
</script>

<style scoped>
input {
  background-color: grey;
  color: black;
  border: 1px solid black;
  padding: auto;
}

h1 {
  font-size: 45px;
  text-align: center;
}

table {
  margin-left: auto;
  margin-right: auto;
}

table,
th,
td {
  border: 1px solid black;
  border-collapse: collapse;
}

th,
td {
  padding: 5px;
  text-align: center;
  background-color: gray;
  color: black;
}

.select {
  text-align: center;
}

.top {
  background-color: rgba(60, 60, 60, 0.29) !important;
}
</style>
